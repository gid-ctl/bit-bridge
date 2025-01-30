# BitBridge Options Protocol

BitBridge Options is a decentralized Bitcoin-native options trading protocol built on the Stacks blockchain. It enables secure and transparent options trading using Bitcoin as the underlying asset, leveraging the SIP-010 token standard for seamless integration with the Bitcoin and Stacks ecosystems.

## Overview

The protocol provides a robust infrastructure for writing, buying, and exercising options contracts in a fully decentralized manner. All options are fully collateralized to ensure trustless trading and minimize counterparty risk.

### Key Features

- **Fully Collateralized Options**: All written options must be backed by sufficient collateral
- **Automated Settlement**: Trustless exercise and settlement of options
- **Dual Option Types**: Support for both CALL and PUT options
- **Price Oracle Integration**: Reliable price feeds for accurate BTC pricing
- **Position Management**: Comprehensive tracking of written and held options
- **SIP-010 Compliance**: Full compatibility with Stacks token standard
- **Protocol Security**: Built-in safeguards and validation checks

## Smart Contract Architecture

### Core Components

1. **Options Management**

   - Unique option ID system
   - Full option state tracking
   - Automated collateral management
   - Exercise mechanics for both CALL and PUT options

2. **Position Tracking**

   - User position mapping
   - Written options tracking
   - Held options tracking
   - Collateral accounting

3. **Price Oracle System**
   - Price feed management
   - Timestamp validation
   - Multi-symbol support
   - Source authentication

### Data Structures

#### Option Contract

```clarity
{
    writer: principal,
    holder: (optional principal),
    collateral-amount: uint,
    strike-price: uint,
    premium: uint,
    expiry: uint,
    is-exercised: bool,
    option-type: (string-ascii 4),
    state: (string-ascii 9)
}
```

#### User Position

```clarity
{
    written-options: (list 10 uint),
    held-options: (list 10 uint),
    total-collateral-locked: uint
}
```

## Core Functions

### Writing Options

```clarity
(write-option
    (token <sip-010-trait>)
    (collateral-amount uint)
    (strike-price uint)
    (premium uint)
    (expiry uint)
    (option-type (string-ascii 4)))
```

- Creates a new option contract
- Locks collateral from the writer
- Updates writer's position

### Buying Options

```clarity
(buy-option
    (token <sip-010-trait>)
    (option-id uint))
```

- Purchases an existing option
- Transfers premium to writer
- Updates buyer's position

### Exercising Options

```clarity
(exercise-option
    (token <sip-010-trait>)
    (option-id uint))
```

- Executes option contract
- Calculates and transfers payout
- Updates option state

## Security Features

### Error Handling

- Comprehensive error codes
- Input validation
- State checks
- Authorization verification

### Access Control

- Contract owner management
- Token whitelist system
- Symbol allowlist
- Critical asset protection

### Validation Checks

- Token validation
- Address validation
- Symbol validation
- Timestamp validation
- Price validation

## Protocol Parameters

### Fee Structure

- Protocol fee rate (basis points)
- Maximum fee rate: 10% (1000 basis points)
- Configurable by contract owner

### Collateral Requirements

- CALL options: Collateral ≥ Strike Price
- PUT options: Collateral ≥ (Strike Price × Current Price Factor)

## Administrative Functions

### Protocol Management

```clarity
(set-protocol-fee-rate (new-rate uint))
(set-approved-token (token principal) (approved bool))
(set-allowed-symbol (symbol (string-ascii 10)) (allowed bool))
```

### Price Feed Management

```clarity
(update-price-feed
    (symbol (string-ascii 10))
    (price uint)
    (timestamp uint))
```

## Integration Guidelines

### Token Integration

1. Token must implement SIP-010 trait
2. Token must be approved by protocol admin
3. Token must have sufficient precision (recommended: 8 decimals)

### Price Feed Integration

1. Symbol must be allowlisted
2. Price updates must have valid timestamps
3. Prices must be non-zero positive integers

## Error Codes

| Code | Description             |
| ---- | ----------------------- |
| 1000 | Not authorized          |
| 1001 | Insufficient balance    |
| 1002 | Invalid expiry          |
| 1003 | Invalid strike price    |
| 1004 | Option not found        |
| 1005 | Option expired          |
| 1006 | Insufficient collateral |
| 1007 | Already exercised       |
| 1008 | Invalid premium         |
| 1009 | Invalid token           |
| 1010 | Invalid symbol          |
| 1011 | Invalid timestamp       |
| 1012 | Invalid address         |
| 1013 | Zero address            |
| 1014 | Empty symbol            |

## Best Practices

1. **Option Writers**

   - Ensure sufficient collateral
   - Set reasonable expiry dates
   - Monitor option states

2. **Option Buyers**

   - Verify option parameters
   - Check expiration dates
   - Understand exercise conditions

3. **Integration**
   - Implement proper error handling
   - Validate all inputs
   - Monitor price feeds
