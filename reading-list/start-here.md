# Start Here: Post-Quantum Bitcoin Readiness

**Status:** Early curated learning path  
**Last checked:** 2026-07-05  
**Audience:** Bitcoin developers, educators, wallet builders, researchers, and technically curious Bitcoiners  
**Goal:** Understand Bitcoin's post-quantum risk surface, and learn how to follow current research and proposal discussions.

---

## What this page is

This page is the entry point for studying **post-quantum cryptography in the context of Bitcoin**.

---

## Suggested reading order

1. [Bitcoin Optech: Quantum resistance](https://bitcoinops.org/en/topics/quantum-resistance/)
  **Type:** Bitcoin technical topic page  
  **Difficulty:** Beginner to intermediate  
  **Why read this:** Best high-level Bitcoin-native starting point for understanding how quantum computers relate to Bitcoin's cryptographic assumptions.  
  **Bitcoin relevance:** Helps separate the risks to signatures from the risks to hash functions and proof-of-work.

2. [NIST: Post-Quantum Cryptography Project](https://csrc.nist.gov/projects/post-quantum-cryptography)  
   **Type:** Standards project page  
  **Status:** Active  
  **Difficulty:** Beginner to intermediate  
  **Why read this:** Canonical entry point for NIST's post-quantum cryptography work.  
  **Bitcoin relevance:** Gives the standards background needed to understand candidate PQ signatures and migration discussions.

3. [BIP360: Pay-to-Merkle-Root](https://bips.dev/360/)  
   **Type:** Bitcoin Improvement Proposal  
  **Status:** Draft  
  **Difficulty:** Intermediate to advanced  
  **Why read this:** Proposes a Taproot-like output type with the key-path spend removed.  
  **Bitcoin relevance:** Useful for understanding long-exposure quantum-risk mitigation and script-tree-based migration paths.

4. [BIP361: Post Quantum Migration and Legacy Signature Sunset](https://bips.dev/361/)  
   **Type:** Bitcoin Improvement Proposal  
  **Status:** Draft  
  **Difficulty:** Advanced  
  **Why read this:** Discusses migration incentives and a possible future sunset of legacy ECDSA and Schnorr spends after a PQ output type exists.  
  **Bitcoin relevance:** Important for understanding the social, economic, and governance dimensions of PQ migration.  

5. [Delving Bitcoin: post-quantum tag](https://delvingbitcoin.org/tag/post-quantum/39)  
   **Type:** Discussion index  
  **Status:** Active  
  **Difficulty:** Intermediate to advanced  
  **Why read this:** Tracks current Bitcoin post-quantum discussions, draft ideas, objections, and experimental proposals.  
  **Bitcoin relevance:** Useful for seeing how Bitcoin contributors reason about tradeoffs in real time.

---

## Core concepts to understand early

### 1. Quantum risk is not one single problem

Bitcoin's post-quantum discussion includes several different risk categories:

- **Long-exposure attacks:** attacks against public keys that have already been exposed on-chain for a long time.
- **Short-exposure attacks:** attacks against public keys revealed while a transaction is waiting to confirm.
- **Hash security:** affected differently from discrete-log-based signatures.
- **Wallet migration:** the practical challenge of moving users and UTXOs to safer output types.
- **Consensus change:** the difficult process of introducing new Bitcoin validation rules.
- **Social coordination:** deciding what should happen to old, unmigrated, or abandoned coins.

---

### 2. Bitcoin's switch to PQC

A post-quantum signature scheme must be evaluated not only for cryptographic security, but also for Bitcoin-specific constraints:

- signature size,
- public-key size,
- transaction weight,
- fee impact,
- verification cost,
- denial-of-service risk,
- hardware wallet support,
- backup and recovery,
- script integration,
- wallet compatibility,
- migration incentives,
- activation risk,
- long-term maintainability.
