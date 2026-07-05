# Bitcoin Cryptography Basics

**Status:** draft  
**Last checked:** 2026-07-05  
**Purpose:** Build the Bitcoin-side foundation needed before studying post-quantum Bitcoin readiness.

This page is for readers who already know what Bitcoin is, but need a structured refresher on the cryptographic and transaction-design ideas that matter for post-quantum migration: ECDSA, Schnorr, Taproot, script paths, public key hashes, witness data, address formats, HD wallets, descriptors, and transaction authorization.

---

## Core resources

### [Bitcoin Developer Guide — Transactions](https://developer.bitcoin.org/devguide/transactions.html)

**Type:** Developer guide  
**Difficulty:** Beginner to intermediate  
**Status:** Educational reference  
**Bitcoin relevance:** Introduces transaction inputs, outputs, scripts, unlocking data, and the relationship between outputs and later spends.  
**Why read this:** Post-quantum Bitcoin risk depends heavily on when a public key is committed to an output and when it is revealed during spending. This guide helps readers understand those mechanics before they study address-type exposure.

**Focus questions:**

- What information is placed in a locking script?
- What information is revealed in an unlocking script or witness?
- How does the spender prove authorization?
- Where does the public key appear in common spend types?

---

### [BIP340 — Schnorr Signatures for secp256k1](https://bips.dev/340/)

**Type:** Bitcoin Improvement Proposal  
**Difficulty:** Intermediate to advanced  
**Status:** Final. Deployed as part of Taproot  
**Bitcoin relevance:** Defines the Schnorr signature scheme used by Taproot. Post-quantum discussions compare candidate PQ signature schemes against BIP340 in terms of signature size, verification cost, key exposure, and wallet integration.  
**Why read this:** BIP340 is the baseline for modern Bitcoin signatures. Any proposal for PQ transaction authorization must explain how it differs from or replaces this kind of secp256k1-based authorization.

**Focus questions:**

- What is the public-key format?
- What is the signature size?
- What assumptions does Schnorr rely on?
- How does this compare with ECDSA?
- What breaks if a cryptographically relevant quantum computer can solve elliptic-curve discrete logs?

---

### [BIP341 — Taproot: SegWit version 1 spending rules](https://bips.dev/341/)

**Type:** Bitcoin Improvement Proposal  
**Difficulty:** Intermediate to advanced  
**Status:** Final. Deployed as part of Taproot 
**Bitcoin relevance:** Defines Taproot outputs and the distinction between key-path and script-path spends. Taproot is central to current PQ discussions because key-path spends expose a secp256k1 public key commitment, while script-path-only constructions may offer a different migration surface.  
**Why read this:** Many post-quantum Bitcoin proposals either modify Taproot, build Taproot-like outputs, remove the key path, or add PQ functionality to script paths.

**Focus questions:**

- What is a Taproot output key?
- What is a Taproot key-path spend?
- What is a Taproot script-path spend?
- What is committed to by the Taproot Merkle tree?
- What information is revealed when a script path is used?
- Why might removing the key path matter for long-exposure quantum attacks?

---

### [BIP342 — Validation of Taproot Scripts](https://bips.dev/342/)

**Type:** Bitcoin Improvement Proposal  
**Difficulty:** Advanced  
**Status:** Final. Deployed as part of Taproot  
**Bitcoin relevance:** Defines Tapscript validation rules. PQ signature integration might be proposed through new witness versions, new output types, or future opcode extensions, so readers should understand the current Tapscript baseline.  
**Why read this:** BIP342 helps explain why some PQ proposals prefer new witness versions or new output types rather than simply dropping a large PQ signature into existing script machinery.

**Focus questions:**

- How does Tapscript differ from pre-Taproot script?
- What are OP_SUCCESSx upgrade paths?
- What limits apply to script execution?
- Why might large PQ signatures stress script or witness assumptions?

---

### [Bitcoin Optech — Schnorr signatures topic](https://bitcoinops.org/en/topics/schnorr-signatures/)

**Type:** Bitcoin engineering explainer  
**Difficulty:** Intermediate  
**Status:** Educational 
**Bitcoin relevance:** Provides practical Bitcoin-focused context around Schnorr signatures, their benefits, and related discussions.  
**Why read this:** Useful before reading PQ proposals that compare Schnorr aggregation, Taproot, MuSig-like constructions, and future PQ signature tradeoffs.

---

### [Bitcoin Optech — Taproot topic](https://bitcoinops.org/en/topics/taproot/)

**Type:** Bitcoin engineering explainer  
**Difficulty:** Intermediate  
**Status:** Educational 
**Bitcoin relevance:** Gives a Bitcoin-native overview of Taproot’s deployment, behavior, and related upgrade discussions.  
**Why read this:** Good secondary reading after BIP341 if the BIP text feels too dense.

---

## Wallet and key-management background

### [BIP32 — Hierarchical Deterministic Wallets](https://bips.dev/32/)

**Type:** Bitcoin Improvement Proposal  
**Difficulty:** Intermediate  
**Status:** Implemented  
**Bitcoin relevance:** Defines HD wallet key derivation. PQ readiness discussions must eventually address wallet backups, key derivation, xpub exposure, and how wallet infrastructure would migrate.  
**Why read this:** Public extended keys and descriptors can reveal public-key information. PQ migration is therefore not only a transaction-format problem, it is also a wallet-architecture problem.

**Focus questions:**

- What does an xpub reveal?
- How do wallets derive many receiving keys?
- How would a PQ wallet replace or emulate HD derivation?
- What assumptions are built into backup and recovery flows?

---

### [Bitcoin Core documentation — Descriptors](https://github.com/bitcoin/bitcoin/blob/master/doc/descriptors.md)

**Type:** Bitcoin Core documentation  
**Difficulty:** Intermediate  
**Status:** Maintained with Bitcoin Core  
**Bitcoin relevance:** Output descriptors describe wallet policies and scripts. They matter for understanding how wallets represent spending conditions and how future PQ output types might be described.  
**Why read this:** PQ readiness will need wallet-level standards, not just consensus rules. Descriptors are one place where wallet policy becomes explicit.

---

### [BIP174 — Partially Signed Bitcoin Transaction Format](https://bips.dev/174/)

**Type:** Bitcoin Improvement Proposal  
**Difficulty:** Intermediate  
**Status:** Implemented  
**Bitcoin relevance:** PSBT is central to hardware wallets, multisig coordination, and offline signing. PQ migration would affect signing devices, transaction-size assumptions, and metadata included during signing.  
**Why read this:** Any future PQ wallet will need a signing workflow. PSBT is the current Bitcoin-native way many wallets coordinate signing.

---

## Optional background

### [Mastering Bitcoin — Open source book repository](https://github.com/bitcoinbook/bitcoinbook)

**Type:** Book  
**Difficulty:** Beginner to intermediate  
**Status:** Open-source educational resource  
**Bitcoin relevance:** Good general Bitcoin foundation.  
**Why read this:** Use it to fill gaps on keys, addresses, transactions, scripts, and wallets.

---

### [SEC 2 — Recommended Elliptic Curve Domain Parameters](https://www.secg.org/sec2-v2.pdf)

**Type:** Cryptographic standards document  
**Difficulty:** Advanced  
**Status:** Reference specification  
**Bitcoin relevance:** Bitcoin uses the secp256k1 elliptic curve.  
**Why read this:** Helpful if you want to understand the underlying curve parameters before studying Shor’s algorithm and elliptic-curve discrete-log attacks.
