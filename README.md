# MedTrack Protocol: Bitcoin-Anchored Medical Device Tracking System

![Stacks Layer 2 Shield](https://img.shields.io/badge/Blockchain-Stacks%20L2-blue)
![Clarity Smart Contract](https://img.shields.io/badge/Smart%20Contract-Clarity-orange)

Enterprise-grade solution for medical device lifecycle management with regulatory compliance on Bitcoin via Stacks Layer 2.

## Overview

MedTrack implements a decentralized protocol for tracking medical devices through manufacturing, testing, deployment, and maintenance phases. The system enforces:

- Immutable status transitions with Bitcoin-finalized audit trails
- Multi-standard certification management (FDA/CE/ISO/Safety)
- Role-based access control for manufacturers and regulators
- Compliance with 21 CFR Part 11 and EU MDR 2017/745

## Key Features

### Device Lifecycle Management

- 4-stage state machine: `Manufactured → Testing → Deployed → Maintained`
- 10-entry immutable history log with timestamped transitions
- Device ownership tracking with principal-based authentication

### Certification Engine

- Support for FDA/CE/ISO/Safety certifications
- Regulatory body authorization system
- Certification revocation workflow
- Real-time validation checks

### Audit System

- Bitcoin-anchored transaction proofs via Stacks L2
- Tamper-evident device history records
- Timestamped certification issuance/revocation

### Compliance Framework

- Input validation for medical device IDs (1-1,000,000 range)
- Status transition enforcement
- Regulatory body whitelisting

## Technical Specifications

### Smart Contract Architecture

| Component         | Technology Stack      |
| ----------------- | --------------------- |
| Language          | Clarity v2.1          |
| State Management  | Data Maps/Vars        |
| Authorization     | Principal-based RBAC  |
| Compliance Checks | Pre-condition asserts |

### Data Structures

```clarity
;; Device Tracking
device-details: {
  owner: principal,
  current-status: uint,
  history: (list 10 {status: uint, timestamp: uint})
}

;; Certifications
device-certifications: {
  issuer: principal,
  timestamp: uint,
  valid: bool
}
```

## Installation

### Requirements

- Clarinet v2.0.0+
- Stacks Testnet access

### Deployment

```bash
clarinet contract new medtrack-protocol
cd medtrack-protocol
clarinet requirements check
```

## Usage

### Device Registration

```clarity
;; Register new device (Manufacturer)
(contract-call? .medtrack-protocol register-device u123 DEVICE_STATUS_MANUFACTURED)
```

### Status Update

```clarity
;; Transition to Deployed status
(contract-call? .medtrack-protocol update-device-status u123 DEVICE_STATUS_DEPLOYED)
```

### Certification Management

```clarity
;; Add FDA certification (Regulator)
(contract-call? .medtrack-protocol add-certification u123 CERT_TYPE_FDA)

;; Verify certification
(contract-call? .medtrack-protocol verify-certification u123 CERT_TYPE_FDA)
```

### Audit Trails

```clarity
;; Retrieve full device history
(contract-call? .medtrack-protocol get-device-history u123)
```

## Compliance & Security

### Validation Checks

- Device ID range verification (1-1,000,000)
- Status transition validity
- Certification type validation
- Regulatory body authorization

### Error Handling

| Error Code         | Description               |
| ------------------ | ------------------------- |
| ERR_UNAUTHORIZED   | Invalid permissions       |
| ERR_INVALID_DEVICE | Non-existent device ID    |
| ERR_STATUS_UPDATE  | Illegal status transition |

For full implementation details, refer to the commented contract source code.
