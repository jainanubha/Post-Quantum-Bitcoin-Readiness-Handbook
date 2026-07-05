# Quantum Risk Model for Bitcoin

**Status:** draft  
**Last checked:** 2026-07-05  
**Purpose:** Build a careful Bitcoin-specific model of quantum risk.

---

## Quantum algorithms and cryptographic background

### [Shor — Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer](https://arxiv.org/abs/quant-ph/9508027)

**Type:** Foundational quantum algorithms paper  
**Difficulty:** Advanced  
**Status:** Foundational research  
**Bitcoin relevance:** Explains the family of quantum algorithms behind the threat to discrete-log cryptography.  
**Key takeaway:** The Bitcoin-relevant point is not factoring specifically, but the discrete-log component: secp256k1 ECDSA and Schnorr authorization relies on elliptic-curve discrete-log hardness.

---

### [Grover — A Fast Quantum Mechanical Algorithm for Database Search](https://arxiv.org/abs/quant-ph/9605043)

**Type:** Foundational quantum algorithms paper  
**Difficulty:** Advanced  
**Status:** Foundational research  
**Bitcoin relevance:** Helps explain why hash-based security estimates are reduced quadratically rather than destroyed in the same way as public-key signatures.  
**Key takeaway:** Grover’s algorithm is important for hashes and mining discussions, but it should not be confused with Shor’s attack on ECDSA and Schnorr.

---

## Bitcoin-specific academic risk papers

### [Aggarwal et al. — Quantum attacks on Bitcoin, and how to protect against them](https://arxiv.org/abs/1710.10377)

**Type:** Research paper  
**Difficulty:** Intermediate to advanced  
**Status:** Classic Bitcoin-specific analysis  
**Bitcoin relevance:** Separates quantum threats to Bitcoin’s proof of work from quantum threats to Bitcoin’s elliptic-curve signatures.  
**Key takeaway:** This is one of the best first academic papers for understanding why signatures are usually considered the more direct quantum concern than mining.

---

### [Holmes and Chen — Assessment of Quantum Threat To Bitcoin and Derived Cryptocurrencies](https://eprint.iacr.org/2021/967)

**Type:** Research paper  
**Difficulty:** Intermediate to advanced  
**Status:** Research  
**Bitcoin relevance:** Studies timing and benchmarking questions for quantum attacks against Bitcoin-like cryptocurrencies.  
**Key takeaway:** The feasibility of an attack depends not only on whether Shor’s algorithm exists, but also on circuit speed, confirmation time, public-key exposure, and chain behavior.

---

### [Roetteler, Naehrig, Svore, Lauter — Quantum resource estimates for computing elliptic curve discrete logarithms](https://arxiv.org/abs/1706.06752)

**Type:** Research paper  
**Difficulty:** Advanced  
**Status:** Foundational resource-estimation paper  
**Bitcoin relevance:** Gives resource estimates for quantum attacks on elliptic-curve discrete logs.  
**Key takeaway:** Useful for understanding how researchers estimate the scale of quantum hardware needed for ECDLP attacks. Read as background, not as a direct Bitcoin migration timeline.

---

### [Google Quantum AI et al. — Securing Elliptic Curve Cryptocurrencies against Quantum Vulnerabilities](https://arxiv.org/abs/2603.28846)

**Type:** Whitepaper
**Difficulty:** Advanced  
**Status:** Recent research 
**Bitcoin relevance:** Gives updated resource estimates and discusses cryptocurrency-specific exposure and mitigation.  
**Key takeaway:** Important for current discussions, especially short-exposure and mempool-risk analysis. Because it is recent and model-dependent, record assumptions separately from conclusions.

Related explainer:

- [Google Research blog — Safeguarding cryptocurrency by disclosing quantum vulnerabilities responsibly](https://research.google/blog/safeguarding-cryptocurrency-by-disclosing-quantum-vulnerabilities-responsibly/)

---

## Mining versus signatures

### [Dallaire-Demers — Kardashev scale Quantum Computing for Bitcoin Mining](https://arxiv.org/abs/2603.25519)

**Type:** Preprint  
**Difficulty:** Advanced  
**Status:** Recent research
**Bitcoin relevance:** Focuses on quantum mining, which is often confused with signature theft.  
**Key takeaway:** Signature attacks and mining attacks are different. This paper is useful for explaining why Grover-based mining advantage must be evaluated with physical cost, fault tolerance, and network timing.

---

### [Gershteyn and Alber — Quantum Horizon: An evaluation of quantum computing as a threat to Bitcoin and Ethereum](https://arxiv.org/abs/2606.14484)

**Type:** Preprint  
**Difficulty:** Intermediate to advanced  
**Status:** Recent preprint
**Bitcoin relevance:** Broad model of quantum risk across Bitcoin and Ethereum, including migration and governance constraints.  
**Key takeaway:** Useful as a recent synthesis, but forecasts and exposure counts should be treated as assumptions to review rather than settled facts.
