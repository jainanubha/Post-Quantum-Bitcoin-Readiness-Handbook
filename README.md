# Post-Quantum-Bitcoin-Readiness-Handbook
An open-source guide for Bitcoin developers, educators, wallet builders, and protocol researchers.

A curated open-source reading list and learning path for studying post-quantum cryptography in the context of Bitcoin.

This repository tracks research papers, Bitcoin Improvement Proposals, Delving Bitcoin discussions, bitcoin-dev threads, standards documents, implementation resources, and educational explainers related to Bitcoin’s quantum-risk surface.

The goal is not to promote a specific consensus change. The goal is to help Bitcoin developers, educators, wallet builders, and researchers understand the design space: public-key exposure, long-exposure and short-exposure attacks, NIST PQC standards, post-quantum signature tradeoffs, transaction weight, wallet migration, and active Bitcoin proposal debates.

## Repository structure

- `handbook/` — original handbook chapters
- `reading-list/` — curated learning resources
- `tables/` — comparison and exposure matrices
- `labs/` — developer labs and experiment designs
- `proposal-tracker.md` — Bitcoin PQ proposal tracking
- `glossary.md` — project terminology

## Handbook table of contents

### Part I — Bitcoin Cryptography and Quantum Threats

- [Chapter 01 — Introduction](handbook/01-introduction.md)
- [Chapter 02 — Why Post-Quantum Readiness Matters](handbook/02-why-post-quantum-readiness-matters.md)
- [Chapter 03 — Bitcoin Cryptography Refresher](handbook/03-bitcoin-cryptography-refresher.md)
- [Chapter 04 — Shor's Algorithm and the Discrete-Log Threat](handbook/04-shors-algorithm-and-discrete-log-threat.md)
- [Chapter 05 — Grover's Algorithm and Bitcoin Hashes](handbook/05-grovers-algorithm-and-bitcoin-hashes.md)

### Part II — Bitcoin's Quantum Risk Surface

- [Chapter 06 — Bitcoin Address Types and Public-Key Exposure](handbook/06-bitcoin-address-types-and-public-key-exposure.md)
- [Chapter 07 — Long-Exposure and Short-Exposure Attacks](handbook/07-long-exposure-vs-short-exposure-attacks.md)
- [Chapter 08 — Taproot Key-Path and Script-Path Considerations](handbook/08-taproot-key-path-and-script-path-considerations.md)

### Part III — Post-Quantum Cryptography and Bitcoin

- [Chapter 09 — NIST Post-Quantum Cryptography Standards](handbook/09-NIST%20Post-Quantum%20Cryptography%20Standards/)
- [Chapter 10 — Post-Quantum Signature Schemes and Bitcoin Constraints](handbook/10-Post-Quantum%20Signature%20Schemes%20and%20Bitcoin%20Constraints/)
- [Chapter 11 — Transaction Weight, Fees and Block-Space Implications](handbook/11-Transaction%20Weight%2C%20Fees%20and%20Block-Space%20Implications/)

### Part IV — Bitcoin Post-Quantum Proposals

- Chapter 12 — BIP360: Pay-to-Merkle-Root (P2MR)
- Chapter 13 — BIP361: Post-Quantum Migration and Legacy-Signature Sunset

### Part V — Bitcoin Post-Quantum Readiness (Planned)

- Chapter 14 — Wallet, Exchange and Custodian Readiness
- Chapter 15 — Open Research Problems and Contribution Opportunities

## Reference Table

### Quantum risk and public-key exposure

- [Address-Type Exposure Matrix](tables/address-type-exposure-matrix.md)

### Post-quantum signature schemes

- [Signature-Size Comparison](tables/signature-size-comparison.md)
- [Post-Quantum Signature Evaluation Matrix](tables/pq-signature-evaluation-matrix.md)
- [Bitcoin Cryptographic Dependencies](tables/bitcoin-cryptographic-dependencies.md)

### Transaction weight and economic implications

- [Post-Quantum Transaction-Weight Comparison](tables/pq-transaction-weight-comparison.md)
- [Fee Sensitivity Analysis](tables/pq-fee-sensitivity.md)
- [Block-Space and Migration Scenarios](tables/pq-block-space-and-migration-scenarios.md)

### Bitcoin post-quantum proposals and migration

- [P2TR, P2WSH and P2MR Comparison](tables/p2tr-p2wsh-p2mr-comparison.md)
- [BIP360 and BIP361 Comparison](tables/bip360-bip361-comparison.md)
- [Post-Quantum Migration Strategy Comparison](tables/pq-migration-strategy-comparison.md)
- [Legacy UTXO Migration and Recovery Matrix](tables/legacy-utxo-migration-and-recovery-matrix.md)

## Research and Proposal Tracking

- [Bitcoin Post-Quantum Proposal Tracker](proposal-tracker.md)
- [Open Research Questions](research/open-questions.md)
- [Bitcoin Post-Quantum Proposals and Discussions](reading-list/bitcoin-post-quantum-proposals-and-discussions.md)
- [Academic Papers](reading-list/academic-papers.md)

## Start here

If you are new to the topic, begin with:

1. `reading-list/start-here.md`
2. `reading-list/bitcoin-cryptography-basics.md`
3. `reading-list/quantum-risk-model-for-bitcoin.md`
4. `reading-list/pqc-standards.md`
5. `reading-list/bitcoin-post-quantum-proposals-and-discussions.md`

## Current status

The handbook is being developed incrementally through research, technical documentation and open-source community review.

Month 1 — August 2026: Repository setup, scope, contribution guidelines and introductory chapters.

Month 2 — September 2026: Bitcoin cryptographic foundations, quantum-risk analysis and public-key exposure.

Month 3 — October 2026: NIST post-quantum standards, signature-scheme evaluation, transaction weight, fees and block-space analysis.

Month 4 — November 2026: BIP360, BIP361, post-quantum migration strategies, legacy UTXO recovery, comparative proposal analysis and expansion of the Bitcoin post-quantum proposal tracker.

## How to contribute

See `CONTRIBUTING.md`.
