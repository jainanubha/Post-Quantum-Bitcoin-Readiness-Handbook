# Academic Papers

**Status:** draft  
**Last checked:** 2026-07-05  
**Purpose:** Curate research papers useful for studying post-quantum cryptography and quantum risk in the context of Bitcoin.

---

## Bitcoin-specific quantum risk

### [Aggarwal, Brennen, Lee, Santha, Tomamichel — Quantum attacks on Bitcoin, and how to protect against them](https://arxiv.org/abs/1710.10377)

**Type:** Research paper  
**Year:** 2017 preprint / 2018 publication context  
**Difficulty:** Intermediate to advanced  
**Bitcoin relevance:** One of the classic Bitcoin-specific analyses of quantum attacks.  
**Key takeaway:** Distinguishes the risk to proof-of-work from the risk to elliptic-curve signatures. Useful first paper for anyone studying Bitcoin PQ readiness.

**Read for:**

- Why ECDSA signatures are a more direct quantum concern than mining.
- How confirmation time affects attack feasibility.
- Why post-quantum signatures create efficiency tradeoffs.
- How to frame Bitcoin-specific risk rather than generic PQC risk.

---

### [Holmes and Chen — Assessment of Quantum Threat To Bitcoin and Derived Cryptocurrencies](https://eprint.iacr.org/2021/967)

**Type:** Research paper  
**Year:** 2021  
**Difficulty:** Intermediate to advanced  
**Bitcoin relevance:** Studies when a quantum computer might be powerful and fast enough to threaten Bitcoin-like cryptocurrencies.  
**Key takeaway:** Attack feasibility depends on both cryptographic vulnerability and timing constraints such as block interval and transaction confirmation behavior.

**Read for:**

- Timing-based threat models.
- Cryptocurrency-specific comparison methodology.
- How block interval and public-key exposure affect risk.
- Threat assessment rather than protocol design.

---

### [Stewart et al. — Committing to Quantum Resistance: A Slow Defence for Bitcoin against a Fast Quantum Computing Attack](https://eprint.iacr.org/2018/213)

**Type:** Research paper  
**Year:** 2018  
**Difficulty:** Advanced  
**Bitcoin relevance:** Proposes a commit-delay-reveal protocol for migrating funds even if ECDSA is compromised.  
**Key takeaway:** Migration can be designed as a protocol problem.

**Read for:**

- Commit-delay-reveal migration.
- Attack models involving compromised ECDSA.
- How migration can reduce front-running risk.
- Links to later commit-then-reveal discussions.

**Related resources:**

- [Post-Quantum Migrations via Commit-then-Reveal — Delving Bitcoin](https://delvingbitcoin.org/t/post-quantum-migrations-via-commit-then-reveal-ala-bip16-an-alternative-to-sunsetting/2467)
- [Post-Quantum commit/reveal Fawkescoin variant — bitcoin-dev](https://groups.google.com/g/bitcoindev/c/LpWOcXMcvk8)

---

### [Ilie, Knottenbelt, Stewart — Committing to Quantum Resistance, Better](https://ideas.repec.org/h/spr/prbchp/978-3-030-37110-4_9.html)

**Type:** Academic chapter
**Year:** 2020  
**Difficulty:** Advanced  
**Bitcoin relevance:** Follow-up work on speed-and-risk configurable defenses for Bitcoin against quantum attacks.  
**Key takeaway:** Useful continuation of the commit-delay-reveal migration literature.

**Read for:**

- Improved migration design.
- Configurable speed/risk tradeoffs.
- How migration protocols can be compared.

---

## Quantum resource estimates

### [Roetteler, Naehrig, Svore, Lauter — Quantum resource estimates for computing elliptic curve discrete logarithms](https://arxiv.org/abs/1706.06752)

**Type:** Research paper  
**Year:** 2017  
**Difficulty:** Advanced  
**Bitcoin relevance:** Foundational resource-estimation work for quantum attacks on elliptic-curve discrete logs.  
**Key takeaway:** Important background for understanding what it means to estimate the quantum cost of breaking ECC.

**Read for:**

- Logical qubits.
- Toffoli gates.
- Circuit depth.
- Elliptic-curve point addition in reversible circuits.
- Why ECC can be an easier quantum target than RSA at comparable classical security levels.

---

### [Google Quantum AI et al. — Securing Elliptic Curve Cryptocurrencies against Quantum Vulnerabilities](https://arxiv.org/abs/2603.28846)

**Type:** Whitepaper
**Year:** 2026  
**Difficulty:** Advanced  
**Bitcoin relevance:** Recent cryptocurrency-focused resource estimate and mitigation discussion for elliptic-curve cryptocurrencies.  
**Key takeaway:** Useful for current debate because it discusses faster resource estimates and cryptocurrency-specific exposure.

**Read for:**

- Updated resource estimates for ECDLP-256.
- Public-key exposure in cryptocurrency systems.
- Short-exposure attack concerns.
- Mitigation framing and responsible disclosure.
- Distinction between fast-clock and slow-clock quantum architectures.

**Related resource:**

- [Google Research blog — Safeguarding cryptocurrency by disclosing quantum vulnerabilities responsibly](https://research.google/blog/safeguarding-cryptocurrency-by-disclosing-quantum-vulnerabilities-responsibly/)

---

### [Dallaire-Demers — Kardashev scale Quantum Computing for Bitcoin Mining](https://arxiv.org/abs/2603.25519)

**Type:** Preprint  
**Year:** 2026  
**Difficulty:** Advanced  
**Bitcoin relevance:** Focuses on quantum mining and the physical cost of Grover-style mining advantage.  
**Key takeaway:** Helps prevent the common mistake of conflating Shor attacks on signatures with Grover effects on mining.

**Read for:**

- Quantum mining cost modeling.
- Double-SHA256 oracle construction.
- Fault-tolerant physical resource assumptions.
- Energy and fleet-size estimates.
- Why mining risk should be analyzed separately from signature risk.

---

### [Gershteyn and Alber — Quantum Horizon: An evaluation of quantum computing as a threat to Bitcoin and Ethereum](https://arxiv.org/abs/2606.14484)

**Type:** Preprint  
**Year:** 2026  
**Difficulty:** Intermediate to advanced  
**Bitcoin relevance:** Broad threat model covering Bitcoin and Ethereum, including exposure and migration constraints.  
**Key takeaway:** Recent synthesis.

**Read for:**

- Broad threat taxonomy.
- Comparison between Bitcoin and Ethereum.
- Governance and migration constraints.
- Forecast modeling assumptions.
- Exposure classification.

---

## Taproot and commitments

### [Ruffing — The Post-Quantum Security of Bitcoin’s Taproot as a Commitment Scheme](https://eprint.iacr.org/2025/1307)

**Type:** Academic paper  
**Year:** 2025  
**Difficulty:** Advanced  
**Bitcoin relevance:** Directly relevant to script-path-only Taproot and P2MR-style reasoning.  
**Key takeaway:** Important for evaluating proposals that rely on Taproot-like commitments or adding PQ signatures to script paths.

**Read for:**

- Taproot as a commitment scheme.
- Quantum random oracle model assumptions.
- Script-path-only upgrade reasoning.
- Relationship to BIP360/P2MR discussions.

**Related discussion:**

- [bitcoin-dev — Taproot is post-quantum secure when restricted to script-path spends](https://groups.google.com/g/bitcoindev/c/ydE5u5C0xVc)

---

## PQC signature foundations

### [Bernstein et al. — The SPHINCS+ Signature Framework](https://eprint.iacr.org/2019/1086)

**Type:** Research paper  
**Year:** 2019  
**Difficulty:** Advanced  
**Bitcoin relevance:** Provides background on SPHINCS+, the basis for NIST’s SLH-DSA.  
**Key takeaway:** Useful for understanding stateless hash-based signatures and why their signatures are large but conservative.

---

### [CRYSTALS-Dilithium papers and specification](https://pq-crystals.org/dilithium/)

**Type:** Scheme papers
**Difficulty:** Advanced  
**Bitcoin relevance:** Dilithium is the family behind ML-DSA, which appears in Bitcoin PQ proposal discussions.

---

## Broader PQC and blockchain references

### [PQCrypto.org resources](https://pqcrypto.org/)

**Type:** Resource hub
**Bitcoin relevance:** Provides broad PQC background outside Bitcoin.  
**Key takeaway:** Useful when you need context on PQC families beyond currently active Bitcoin discussions.

---

### [Bernstein, Buchmann, Dahmen — Post-Quantum Cryptography](https://link.springer.com/book/10.1007/978-3-540-88702-7)

**Type:** Book  
**Difficulty:** Advanced  
**Bitcoin relevance:** General PQC reference.  
**Key takeaway:** Not Bitcoin-specific, but useful for developing a deeper cryptographic foundation.
