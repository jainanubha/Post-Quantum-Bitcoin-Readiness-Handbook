# 03 — Post-Quantum Cryptography Standards

**Status:** draft  
**Last checked:** 2026-07-05  
**Purpose:** Track post-quantum cryptography standards and background material relevant to Bitcoin developers.

This page focuses on post-quantum cryptographic primitives, especially digital signatures. Bitcoin transaction authorization is a signature problem, not a key-encapsulation problem, so this page distinguishes:

- **KEMs** such as ML-KEM, which are important for internet protocols and encrypted communication but are not directly Bitcoin transaction signatures.
- **Digital signatures** such as ML-DSA and SLH-DSA, which are directly relevant to possible future Bitcoin spending authorization.
- **Stateful hash-based signatures** such as XMSS and LMS, HSS, which may have attractive size properties but dangerous state-management requirements.
- **Candidate or future standards** such as Falcon, FN-DSA and other additional digital-signature schemes.

---

## NIST primary sources

### [NIST Post-Quantum Cryptography project](https://csrc.nist.gov/projects/post-quantum-cryptography)

**Type:** Standards project page  
**Difficulty:** Beginner to intermediate  
**Status:** Primary source  
**Bitcoin relevance:** Canonical overview of NIST’s PQC standardization process and current standardized algorithms.  
**Key takeaway:** Use this page as the authoritative starting point for the state of NIST PQC standardization.

---

### [FIPS 204 — Module-Lattice-Based Digital Signature Standard, ML-DSA](https://csrc.nist.gov/pubs/fips/204/final)

**Type:** NIST standard  
**Difficulty:** Advanced  
**Status:** Final  
**Bitcoin relevance:** ML-DSA is a standardized post-quantum digital signature scheme. It is directly relevant to discussions about PQ Bitcoin transaction authorization.  
**Key takeaway:** ML-DSA is one of the main standardized signature families that Bitcoin proposals may compare against. Bitcoin-specific adoption would still require analysis of signature size, public-key size, validation cost, implementation safety, sighash design, and consensus deployment.

**Focus questions:**

- What are the ML-DSA parameter sets?
- What are the public key and signature sizes?
- What are the verification costs?
- What implementation assumptions matter?
- Why is standardization not the same as Bitcoin readiness?

---

### [FIPS 205 — Stateless Hash-Based Digital Signature Standard, SLH-DSA](https://csrc.nist.gov/pubs/fips/205/final)

**Type:** NIST standard  
**Difficulty:** Advanced  
**Status:** Final  
**Bitcoin relevance:** SLH-DSA is a standardized stateless hash-based signature scheme based on SPHINCS+.  
**Key takeaway:** SLH-DSA relies on hash-based assumptions, but its signatures are generally large. For Bitcoin, this raises transaction weight, fee, and validation considerations.

**Focus questions:**

- Why is statelessness operationally valuable?
- Why are hash-based signatures attractive for conservative security?
- How large are SLH-DSA signatures compared with Schnorr?
- Would fee pressure make SLH-DSA difficult for normal Bitcoin use?

---

### [FIPS 203 — Module-Lattice-Based Key-Encapsulation Mechanism Standard, ML-KEM](https://csrc.nist.gov/pubs/fips/203/final)

**Type:** NIST standard  
**Difficulty:** Advanced  
**Status:** Final  
**Bitcoin relevance:** ML-KEM is not a Bitcoin transaction signature scheme, but it matters for network encryption, wallet communication, Lightning communication, and general PQ migration literacy.

**Focus questions:**

- What problem does a KEM solve?
- Why is a KEM not a drop-in replacement for ECDSA or Schnorr?
- Where might KEMs matter in the Bitcoin ecosystem outside L1 transaction signing?

---

### [NIST IR 8547 — Transition to Post-Quantum Cryptography Standards](https://csrc.nist.gov/pubs/ir/8547/ipd)

**Type:** NIST migration guidance  
**Difficulty:** Intermediate  
**Status:** Initial public draft
**Bitcoin relevance:** Useful for thinking about crypto inventory, migration planning, and phase-out of vulnerable algorithms in large systems.  
**Key takeaway:** Migration planning concepts — inventory, risk prioritization, timelines, interoperability.

---

### [NIST SP 800-208 — Recommendation for Stateful Hash-Based Signature Schemes](https://csrc.nist.gov/pubs/sp/800/208/final)

**Type:** NIST recommendation  
**Difficulty:** Advanced  
**Status:** Final  
**Bitcoin relevance:** Covers stateful hash-based signature schemes such as LMS and XMSS. These may be discussed for Bitcoin because they can be smaller than stateless signatures.  
**Key takeaway:** Stateful schemes require strict non-reuse of signing state. In Bitcoin, this interacts badly with wallet backups, RBF, multisig coordination, accidental duplicate signing, hardware wallet state, and recovery.

---

## Hash-based signature references

### [RFC 8391 — XMSS: eXtended Merkle Signature Scheme](https://datatracker.ietf.org/doc/html/rfc8391)

**Type:** IETF RFC  
**Difficulty:** Advanced  
**Status:** RFC  
**Bitcoin relevance:** XMSS is a stateful hash-based signature scheme.  
**Key takeaway:** Useful for understanding one-time and few-time signing, Merkle authentication paths, and why state management is a serious wallet-design issue.

---

### [RFC 8554 — Leighton-Micali Hash-Based Signatures](https://www.rfc-editor.org/rfc/rfc8554)

**Type:** IETF RFC  
**Difficulty:** Advanced  
**Status:** RFC  
**Bitcoin relevance:** LMS and HSS is another stateful hash-based signature family.  
**Key takeaway:** Good comparison point for proposals that want conservative hash-based security but must manage state safely.

---

### [SPHINCS+ official site](https://sphincs.org/)

**Type:** Scheme website and implementation reference  
**Difficulty:** Intermediate to advanced  
**Status:** Scheme
**Bitcoin relevance:** SPHINCS+ is the basis for SLH-DSA.  
**Key takeaway:** Useful for understanding how stateless hash-based signatures achieve post-quantum security and what costs they introduce.

---

## Lattice-based signature references

### [CRYSTALS-Dilithium official site](https://pq-crystals.org/dilithium/)

**Type:** Scheme website
**Difficulty:** Intermediate to advanced  
**Status:** Scheme background  
**Bitcoin relevance:** Dilithium is the pre-standardization name of ML-DSA.  
**Key takeaway:** Useful for learning the family of lattice-based signatures that became ML-DSA.

---

### [Cloudflare — Deep dive into a post-quantum signature scheme](https://blog.cloudflare.com/post-quantum-signatures/)

**Type:** Technical explainer  
**Difficulty:** Intermediate  
**Status:** Educational article  
**Bitcoin relevance:** Helps Bitcoin engineers understand lattice-based signature ideas before reading dense standards.  
**Key takeaway:** Use as a bridge between high-level PQC descriptions and the FIPS specification.

---

## General PQC background

### [PQCrypto.org](https://pqcrypto.org/)

**Type:** Educational resource hub  
**Difficulty:** Beginner to advanced  
**Status:** Background reference  
**Bitcoin relevance:** Provides conceptual background on PQC families: lattice-based, code-based, hash-based, multivariate, and isogeny-based cryptography.  
**Key takeaway:** A broader map of PQC beyond the NIST standards.

---

### [Bernstein, Buchmann, Dahmen — Post-Quantum Cryptography](https://link.springer.com/book/10.1007/978-3-540-88702-7)

**Type:** Book  
**Difficulty:** Advanced  
**Status:** Reference book  
**Bitcoin relevance:** Gives a broad cryptographic foundation for PQC families and design assumptions.  
**Key takeaway:** Useful for deeper study (not Bitcoin-specific).

---

### [NIST Additional Digital Signature Schemes project](https://csrc.nist.gov/projects/pqc-dig-sig)

**Type:** Standards project page  
**Difficulty:** Intermediate  
**Status:** Ongoing standardization effort  
**Bitcoin relevance:** Bitcoin may not want to commit prematurely to one signature family. Tracking additional signature candidates matters for long-term crypto-agility discussions.
