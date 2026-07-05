# Implementation Resources

**Status:** draft  
**Last checked:** 2026-07-05  
**Purpose:** Track software libraries, reference implementations, test frameworks, and experimental resources useful for learning about post-quantum cryptography in Bitcoin.

---

## Core PQC implementation resources

### [Open Quantum Safe — liboqs](https://openquantumsafe.org/liboqs/)

**Type:** Open-source C library  
**Difficulty:** Intermediate to advanced  
**Status:** Active PQC prototyping library  
**Bitcoin relevance:** Useful for experimenting with ML-KEM, ML-DSA, SLH-DSA, and other PQ algorithms.  
**Key takeaway:** liboqs is good for prototyping and benchmarking.

**Use for:**

- Local PQ signature experiments.
- Measuring signature and public-key sizes.
- Benchmarking signing and verification.
- Understanding algorithm APIs.

---

### [Open Quantum Safe — liboqs GitHub repository](https://github.com/open-quantum-safe/liboqs)

**Type:** Source repository  
**Difficulty:** Advanced  
**Status:** Active development  
**Bitcoin relevance:** Useful for inspecting supported algorithms, build options, tests, and benchmark tooling.  
**Key takeaway:** If a Bitcoin PQ proposal uses liboqs, reviewers should ask whether that is only for prototyping or intended as a long-term dependency.

**Review questions:**

- Which algorithms are enabled?
- Which algorithms are NIST standardized?
- Which are experimental?
- What version is pinned?
- How are test vectors handled?
- What portability assumptions exist?

---

### [Open Quantum Safe — supported algorithm list](https://openquantumsafe.org/liboqs/algorithms/)

**Type:** Algorithm support matrix  
**Difficulty:** Intermediate  
**Status:** Maintained with liboqs  
**Bitcoin relevance:** Helps distinguish standardized algorithms from experimental ones.  
**Key takeaway:** Good reference when reading proposals that mention ML-DSA, SLH-DSA, Falcon/FN-DSA, HQC, or other algorithm names.

---

### [Open Quantum Safe — oqs-provider](https://github.com/open-quantum-safe/oqs-provider)

**Type:** OpenSSL 3 provider  
**Difficulty:** Intermediate to advanced  
**Status:** Active  
**Bitcoin relevance:** Useful for experimenting with PQ/hybrid cryptography in TLS-like settings, wallet communication, and educational labs.  
**Key takeaway:** Relevant mostly outside Bitcoin L1 consensus. Helpful for understanding PQ migration in communications infrastructure.

---

### [PQClean](https://github.com/PQClean/PQClean)

**Type:** Clean implementation repository  
**Difficulty:** Advanced  
**Status:** Active, reference-quality implementation project  
**Bitcoin relevance:** Useful for studying portable, tested PQC implementations and for comparing implementation styles.  
**Key takeaway:** PQClean is valuable for implementation hygiene and educational experiments, but using any implementation in Bitcoin consensus would still require deep review.

**Use for:**

- Reading clean C implementations.
- Comparing test-vector practices.
- Learning implementation constraints.
- Generating wrappers in other languages.

---

## Scheme-specific implementation references

### [SPHINCS+ reference implementation](https://github.com/sphincs/sphincsplus)

**Type:** Reference implementation  
**Difficulty:** Advanced  
**Status:** Scheme reference  
**Bitcoin relevance:** Useful for understanding the construction behind SLH-DSA and hash-based signature costs.  
**Key takeaway:** Helpful for educational comparison with ML-DSA, WOTS, XMSS, and LMS.

---

### [CRYSTALS-Dilithium official site](https://pq-crystals.org/dilithium/)

**Type:** Scheme reference and implementation information  
**Difficulty:** Advanced  
**Status:** Pre-standardization family behind ML-DSA  
**Bitcoin relevance:** Useful when studying ML-DSA-style proposals.  
**Key takeaway:** Good background for understanding lattice-based signature design.

---

### [Falcon official site](https://falcon-sign.info/)

**Type:** Scheme reference  
**Difficulty:** Advanced  
**Status:** Selected by NIST for future standardization as FN-DSA.
**Bitcoin relevance:** Falcon, FN-DSA appears in discussions because of compact signatures, but implementation and determinism concerns are important for consensus systems.  
**Key takeaway:** Useful comparison point, not a default recommendation.

---

## Bitcoin proposal implementations and experiments

### [Witness Version 3 ML-DSA-65 reference implementation discussion](https://delvingbitcoin.org/t/bip-draft-witness-version-3-ml-dsa-65-post-quantum-key-path-spending/2422)

**Type:** Delving Bitcoin discussion with code links  
**Difficulty:** Advanced  
**Status:** Experimental  
**Bitcoin relevance:** Concrete prototype of a possible new witness version using ML-DSA-65.  
**Key takeaway:** Use this to study how a PQ signature proposal interacts with sighash, witness structure, transaction weight, validation cost, and Bitcoin Core integration.

**Review questions:**

- Is the implementation consensus-safe?
- What cryptographic library is used?
- How are test vectors handled?
- What are the benchmark results?
- How does the design avoid cross-version signature confusion?
- How are large witnesses accounted for?

---

### [P2WOTS proposal code discussion](https://delvingbitcoin.org/t/p2wots-post-quantum-utxo-winternitz-signatures/2530)

**Type:** Delving Bitcoin discussion with code link  
**Difficulty:** Advanced  
**Status:** Experimental  
**Bitcoin relevance:** Useful for exploring WOTS-style UTXO signatures and one-time-signature hazards.  
**Key takeaway:** Good educational example of why Bitcoin wallet behavior matters as much as signature mathematics.

---

### [BIP360 reference and project site](https://bip360.org/)

**Type:** Proposal project site  
**Difficulty:** Intermediate to advanced  
**Status:** Draft proposal resource  
**Bitcoin relevance:** Explains P2MR and links related work.  
**Key takeaway:** Use the BIP text as the primary source, and the project site as an explainer.

Primary BIP link:

- [BIP360 on bips.dev](https://bips.dev/360/)

---

## Bitcoin development environments for labs

### [Bitcoin Core repository](https://github.com/bitcoin/bitcoin)

**Type:** Bitcoin node implementation  
**Difficulty:** Advanced  
**Status:** Production Bitcoin implementation  
**Bitcoin relevance:** Any consensus or policy proposal eventually needs to be understood in relation to Bitcoin Core validation, policy, wallet tooling, and tests.

---

### [Bitcoin Core functional test framework](https://github.com/bitcoin/bitcoin/tree/master/test/functional)

**Type:** Test framework  
**Difficulty:** Advanced  
**Status:** Maintained with Bitcoin Core  
**Bitcoin relevance:** Useful for prototyping transaction and policy behavior in regtest.  
**Key takeaway:** Good place to learn how Bitcoin Core tests consensus and policy behavior.

---

### [rust-bitcoin](https://github.com/rust-bitcoin/rust-bitcoin)

**Type:** Rust Bitcoin library  
**Difficulty:** Intermediate to advanced  
**Status:** Active open-source library  
**Bitcoin relevance:** Useful for parsing, constructing, and analyzing Bitcoin transactions in Rust.  
**Key takeaway:** Good candidate for educational tooling such as transaction-size calculators and output-exposure classifiers.

---

### [BDK — Bitcoin Development Kit](https://github.com/bitcoindevkit/bdk)

**Type:** Wallet development library  
**Difficulty:** Intermediate to advanced  
**Status:** Active open-source wallet library  
**Bitcoin relevance:** Useful for wallet-oriented experiments, transaction construction, descriptors, and migration readiness labs.  
**Key takeaway:** PQ readiness is a wallet problem too. BDK can help build educational examples without starting from scratch.

---

## Suggested lab projects

### Lab 1 — Signature-size comparison

**Goal:** Compare signature and public-key sizes across BIP340 Schnorr, ML-DSA, SLH-DSA, XMSS, LMS, and WOTS-style schemes.

---

### Lab 2 — Transaction-weight estimator

**Goal:** Estimate transaction weight and fee impact for different signature sizes.

---

### Lab 3 — UTXO exposure classifier

**Goal:** Classify example outputs by public-key exposure.

---

### Lab 4 — Regtest public-key visibility demo

**Goal:** Demonstrate when public keys become visible for different output types.

---

### Lab 5 — Proposal weight reproduction

**Goal:** Reproduce rough transaction-weight estimates from PQ proposal discussions.
