# VRFStack
## Quantum-Resilient Randomness Oracle for Stacks Blockchain

VRFStack is an advanced Verifiable Random Function implementation designed specifically for the Stacks blockchain ecosystem. This protocol provides cryptographically secure, verifiable randomness through a sophisticated commit-reveal mechanism with entropy optimization and gas-efficient operations.

## Core Features

### Cryptographic Security
- **Commit-Reveal Protocol**: Two-phase randomness generation preventing manipulation
- **SHA-512/256 Entropy Mixing**: Advanced cryptographic hashing for maximum entropy
- **Quantum-Resilient Design**: Future-proof against quantum computing threats
- **Verifiable Outputs**: All randomness can be independently verified

### Gas Optimization
- **Entropy Refresh Intervals**: Intelligent cooldown periods to minimize gas costs
- **Truncated Processing**: Optimized buffer handling for efficient computation
- **Single-Block Operations**: Minimized transaction complexity

### Protocol Management
- **Hibernation Mode**: Administrative pause functionality for maintenance
- **Identity Quarantine**: Address-level access control and security
- **Commitment Windows**: Time-bounded reveal windows (24 hours)
- **Sequence Generation**: Batch randomness generation up to 50 values

## Technical Specifications

### Constants
- **Max Sequence Threshold**: 50 random values per batch
- **Commitment Window**: 144 blocks (~24 hours)
- **Entropy Refresh Interval**: 3 blocks minimum between generations
- **Randomness Ceiling**: 1,000,000 maximum range value

### Error Codes
```clarity
ERR-UNAUTHORIZED-GENESIS (100)        - Unauthorized protocol access
ERR-ENTROPY-BOUNDS-VIOLATION (101)    - Invalid range parameters
ERR-MALFORMED-PARAMETERS (102)        - Invalid function parameters
ERR-SEQUENCE-CAPACITY-EXCEEDED (103)  - Sequence length too large
ERR-TEMPORAL-LOCK-ACTIVE (104)        - Cooldown period active
ERR-ADDRESS-QUARANTINED (105)         - Address is restricted
ERR-INSUFFICIENT-ENTROPY-POOL (106)   - Low entropy condition
ERR-PROTOCOL-HIBERNATION (107)        - Protocol is paused
ERR-COMMITMENT-WINDOW-EXPIRED (108)   - Reveal window closed
ERR-REVELATION-MISMATCH (109)         - Invalid reveal data
ERR-COMMITMENT-HASH_COLLISION (110)   - Invalid commitment hash
ERR-IDENTITY-VALIDATION-FAILED (111)  - Address validation failed
```

## Usage

### Basic Random Number Generation
```clarity
;; Generate a random number
(contract-call? .vrfstack generate-entropy)

;; Generate random number within range
(contract-call? .vrfstack generate-bounded-entropy u1 u100)

;; Generate sequence of random numbers
(contract-call? .vrfstack generate-entropy-sequence u10)
```

### Commit-Reveal Process
```clarity
;; Step 1: Commit to a random value
(let ((commitment (sha256 0x1234567890abcdef)))
  (contract-call? .vrfstack commit-entropy-intention commitment))

;; Step 2: Reveal the original value (within 144 blocks)
(contract-call? .vrfstack reveal-entropy-source 0x1234567890abcdef)
```

### Administrative Functions
```clarity
;; Toggle hibernation mode (owner only)
(contract-call? .vrfstack toggle-protocol-hibernation true)

;; Quarantine an address (owner only)
(contract-call? .vrfstack quarantine-identity 'SP1234567890...)

;; Release quarantined address (owner only)
(contract-call? .vrfstack release-quarantined-identity 'SP1234567890...)
```

## Read-Only Functions

```clarity
;; Inspect current entropy output
(contract-call? .vrfstack inspect-current-entropy)

;; Check commitment record for an address
(contract-call? .vrfstack inspect-commitment-record 'SP1234567890...)

;; Check if address is quarantined
(contract-call? .vrfstack inspect-quarantine-status 'SP1234567890...)
```

## Security Features

### Entropy Sources
- Block hashes from previous blocks
- User-provided commitment values
- Cryptographic mixing functions
- Temporal block height variations

### Attack Resistance
- **Front-running Protection**: Commit-reveal prevents prediction
- **Block Manipulation Resistance**: Multiple entropy sources
- **Temporal Attacks**: Cooldown periods and time windows
- **Access Control**: Owner-managed quarantine system

## 🏗️ Architecture

### State Management
- **Protocol Hibernation State**: Global pause mechanism
- **Current Entropy Output**: Latest generated randomness
- **Master Entropy Seed**: Accumulated entropy state
- **Genesis Height Tracking**: Block height management

### Registry Systems
- **Commitment Registry**: Tracks commit-reveal states
- **Quarantine Registry**: Manages restricted addresses
- **Generation Metrics**: Usage statistics and monitoring

## Gas Optimization Strategies

1. **Reduced Sequence Limits**: Maximum 50 values per batch
2. **Efficient Buffer Operations**: 16-byte truncation for processing
3. **Minimal State Updates**: Optimized variable modifications
4. **Smart Cooldowns**: Balanced security and usability
5. **Single-Transaction Operations**: Atomic randomness generation

## Protocol Lifecycle

1. **Initialization**: Protocol starts with zero entropy state
2. **First Generation**: Uses block hash as initial entropy
3. **Accumulation**: Each generation adds to entropy pool
4. **Commit-Reveal**: Enhanced entropy through user participation
5. **Continuous Operation**: Maintains entropy state across blocks

## Use Cases

- **Gaming Applications**: Provably fair random mechanics
- **NFT Generation**: Verifiable trait randomization
- **Lottery Systems**: Transparent winner selection
- **Sampling Algorithms**: Statistical random sampling
- **Cryptographic Applications**: Nonce and key generation
- **Governance Systems**: Random committee selection

## Contributing

VRFStack is designed for production use on Stacks mainnet. Contributions should focus on:

- Gas optimization improvements
- Additional entropy sources
- Enhanced security features
- Extended functionality
- Comprehensive testing

