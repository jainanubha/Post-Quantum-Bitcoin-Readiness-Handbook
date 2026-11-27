# Glossary

This glossary defines terminology used throughout the Post-Quantum Bitcoin Readiness Handbook. Definitions emphasize the Bitcoin-specific context of each concept. Detailed explanations and references are provided in the relevant handbook chapters.

## Post-Quantum Cryptography Standards

### Post-Quantum Cryptography (PQC)

Cryptographic algorithms designed to resist attacks from both classical and quantum computers under currently understood computational assumptions.

### NIST Security Categories

Five security categories defined by NIST using reference computational problems involving exhaustive key search and hash collisions. These categories provide a framework for evaluating post-quantum cryptographic security and should not be interpreted as direct numerical estimates of quantum security bits.

### Key-Encapsulation Mechanism (KEM)

A cryptographic primitive that enables two parties to establish a shared secret using public-key cryptography. A KEM does not provide the publicly verifiable digital signatures required for Bitcoin transaction authorization.

### ML-KEM

Module-Lattice-Based Key-Encapsulation Mechanism standardized in NIST FIPS 203 and derived from CRYSTALS-Kyber.

### ML-DSA

Module-Lattice-Based Digital Signature Algorithm standardized in NIST FIPS 204 and derived from CRYSTALS-Dilithium.

### SLH-DSA

Stateless Hash-Based Digital Signature Algorithm standardized in NIST FIPS 205 and derived from SPHINCS+.

## Signature Schemes and Security

### Stateful Signature Scheme

A signature scheme that requires the signer to maintain and correctly update signing state to preserve security. Restoring obsolete signing state or independently using duplicated state may compromise security.

### Stateless Signature Scheme

A signature scheme that does not require a persistent signing counter or comparable mutable state to prevent reuse of one-time signing components. Statelessness does not imply unlimited signing capacity or the absence of other operational requirements.

### Threshold Signature

A digital signature generated jointly by an authorized subset of participants holding shares of signing authority under a group public key.

### Signature Aggregation

A cryptographic technique for combining multiple signatures into a compact representation that authenticates the associated messages and public keys.

### Batch Verification

A technique for verifying multiple signatures together more efficiently than independent verification. Batch verification does not necessarily reduce the serialized size of the signatures.

## Bitcoin Transaction Accounting

### CompactSize

A variable-length unsigned integer encoding used in Bitcoin serialization to represent values such as input counts, output counts and witness-item lengths.

### Stripped Transaction Size

The serialized transaction size in bytes excluding witness serialization, including the SegWit marker and flag.

### Transaction Weight

A Bitcoin transaction resource-accounting measure defined by BIP141. Transaction weight equals three times the stripped transaction size plus the total serialized transaction size.

### Weight Unit (WU)

The unit used to express transaction and block weight under Bitcoin's SegWit accounting rules.

### Virtual Size (vsize)

The transaction weight divided by four and rounded upward to the nearest integer, as defined in BIP141.

### Virtual Byte (vB)

The unit used to express virtual transaction size.

### Witness Discount

The difference in weight accounting between witness serialization and stripped transaction data. Each additional witness byte contributes one weight unit, while each stripped transaction byte contributes four weight units.

### Fee Rate

The transaction fee relative to a transaction's size, commonly expressed in satoshis per virtual byte (sat/vB).

## Post-Quantum Migration

### Cryptographic Agility

The ability of a system to introduce, replace or retire cryptographic algorithms while maintaining security and interoperability.

### Hybrid Signature Construction

An authorization construction combining multiple signature mechanisms, potentially based on different mathematical assumptions. Its security depends on the composition and verification policy.

### Pay-to-Merkle-Root (P2MR)

A proposed Bitcoin output construction described in BIP360 that commits directly to a script-tree Merkle root without providing a Taproot-style elliptic-curve key-path spending mechanism. It is intended to address long-exposure quantum risk but does not independently provide post-quantum signature verification.

### Legacy-Signature Sunset

A proposed transition process for restricting or eventually discontinuing quantum-vulnerable signature mechanisms. BIP361 discusses such a transition in the context of Bitcoin migration.
