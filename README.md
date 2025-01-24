# Bitstack Analytics Protocol

A decentralized analytics protocol enabling transparent data insights through staking, governance, and incentive mechanisms. Built to revolutionize how blockchain data is analyzed and monetized while ensuring fair participation and rewards distribution.

## Overview

The Bitstack Analytics Protocol is a Clarity smart contract that implements a decentralized analytics platform with the following key features:

- Staking mechanism with tiered rewards
- Governance system with proposal creation and voting
- Token-based incentive structure
- Multi-tier user system with escalating benefits
- Flexible staking periods with lock-up bonuses

## Contract Architecture

### Token System

- **ANALYTICS-TOKEN**: Native fungible token for protocol governance and rewards
- **STX**: Used for staking and participation in the protocol

### Tier System

| Tier | Minimum Stake | Reward Multiplier | Features Enabled |
| ---- | ------------- | ----------------- | ---------------- |
| 1    | 1M uSTX       | 1x                | Basic features   |
| 2    | 5M uSTX       | 1.5x              | Enhanced access  |
| 3    | 10M uSTX      | 2x                | Full access      |

### Staking Mechanics

#### Lock Periods and Multipliers

- No lock: 1x multiplier
- 1 month (4,320 blocks): 1.25x multiplier
- 2 months (8,640 blocks): 1.5x multiplier

#### Base Rewards

- 5% base reward rate
- Additional 1% bonus for longer staking periods
- Rewards calculated based on stake amount, time, and multipliers

### Governance System

#### Proposal Creation

- Minimum voting power required: 1M tokens
- Description length: 10-256 characters
- Voting period: 100-2,880 blocks (approximately 1 day)

#### Voting Mechanism

- Weight-based voting system
- Votes proportional to user's voting power
- Minimum votes threshold for proposal validity

## Core Functions

### Staking Operations

```clarity
(define-public (stake-stx (amount uint) (lock-period uint)))
```

Stakes STX tokens with optional lock period

- Parameters:
  - `amount`: Amount of STX to stake
  - `lock-period`: Lock duration in blocks (0, 4320, or 8640)
- Returns: (ok true) on success

```clarity
(define-public (initiate-unstake (amount uint)))
```

Initiates unstaking process

- Parameters:
  - `amount`: Amount of STX to unstake
- Starts 24-hour cooldown period

```clarity
(define-public (complete-unstake))
```

Completes unstaking after cooldown period

- Returns staked STX to user
- Clears staking position

### Governance Functions

```clarity
(define-public (create-proposal (description (string-utf8 256)) (voting-period uint)))
```

Creates new governance proposal

- Parameters:
  - `description`: Proposal description
  - `voting-period`: Duration in blocks
- Returns proposal ID

```clarity
(define-public (vote-on-proposal (proposal-id uint) (vote-for bool)))
```

Casts vote on proposal

- Parameters:
  - `proposal-id`: Target proposal
  - `vote-for`: true for support, false against

### Administrative Functions

```clarity
(define-public (pause-contract))
(define-public (resume-contract))
```

Emergency controls for contract owner

- Pause: Stops new staking operations
- Resume: Re-enables contract functionality

## Data Structures

### User Positions

```clarity
{
    total-collateral: uint,
    total-debt: uint,
    health-factor: uint,
    last-updated: uint,
    stx-staked: uint,
    analytics-tokens: uint,
    voting-power: uint,
    tier-level: uint,
    rewards-multiplier: uint
}
```

### Staking Positions

```clarity
{
    amount: uint,
    start-block: uint,
    last-claim: uint,
    lock-period: uint,
    cooldown-start: (optional uint),
    accumulated-rewards: uint
}
```

### Proposals

```clarity
{
    creator: principal,
    description: (string-utf8 256),
    start-block: uint,
    end-block: uint,
    executed: bool,
    votes-for: uint,
    votes-against: uint,
    minimum-votes: uint
}
```

## Error Codes

| Code | Description                 |
| ---- | --------------------------- |
| 1000 | Not authorized              |
| 1001 | Invalid protocol parameters |
| 1002 | Invalid amount              |
| 1003 | Insufficient STX            |
| 1004 | Cooldown period active      |
| 1005 | No stake found              |
| 1006 | Below minimum stake         |
| 1007 | Contract paused             |

## Security Considerations

1. **Access Control**

   - Contract owner privileges
   - Tier-based access restrictions
   - Voting power requirements

2. **Staking Safety**

   - Minimum stake requirements
   - Cooldown periods
   - Lock period validations

3. **Governance Protection**
   - Minimum voting thresholds
   - Proposal duration limits
   - Description length validation

## Best Practices for Integration

1. **Staking**

   - Verify minimum stake amounts
   - Consider lock periods for maximum rewards
   - Monitor cooldown periods

2. **Governance**

   - Build sufficient voting power before proposals
   - Review voting periods
   - Monitor proposal status

3. **Tier Management**
   - Plan stake amounts for desired tier
   - Calculate reward multipliers
   - Track feature availability

## Development and Testing

1. Clone the repository
2. Deploy contract to Stacks network
3. Initialize contract with `initialize-contract`
4. Test with provided error codes
5. Verify tier system functionality
