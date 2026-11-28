# Bitcoin Post-Quantum Proposals and Discussions

**Purpose:** Track Bitcoin-specific post-quantum proposals, draft BIPs, mailing-list threads, Delving Bitcoin discussions, and design debates.

Many of the listed resources here are drafts or forum discussions.

---

## Curation labels

Use the following labels consistently:

| Label | Meaning |
|---|---|
| `deployed` | Already active in Bitcoin consensus or widely deployed infrastructure. |
| `draft-bip` | Draft BIP or proposal text exists, but not activated. |
| `discussion` | Mailing-list/forum discussion; not a BIP. |
| `research` | Academic or experimental idea. |
| `implementation-experiment` | Code/prototype exists, but not production or consensus-ready. |
| `controversial` | Raises major social, economic, or consensus tradeoffs. |
| `needs-review` | Requires expert review before being treated as serious. |

---

## Draft BIPs

### [BIP360 — Pay-to-Merkle-Root, P2MR](https://bips.dev/360/)

**Type:** Bitcoin Improvement Proposal  
**Labels:** `draft-bip`, `soft-fork`, `long-exposure`, `taproot-adjacent`  
**Difficulty:** Intermediate to advanced  
**Bitcoin relevance:** P2MR is designed as a Taproot-like script-tree output that removes the key-path spend.  
**Key takeaway:** BIP360 is best understood as a proposed first step for long-exposure resistance, not as a complete PQ signature solution.

**What it tries to solve:**

- Long-exposure risk for Taproot-like script-tree outputs.
- Need for a script-tree output that avoids a quantum-vulnerable key path.
- A possible future home for PQ signature validation in script leaves.

**What it does not fully solve:**

- Short-exposure attacks during mempool spend time.
- Legacy coins whose public keys are already visible.
- Choice of a specific PQ signature scheme.
- Full wallet, exchange, and hardware migration.

**Questions to ask while reading:**

- Why remove the key path?
- What functionality is preserved from Taproot?
- How does P2MR compare with P2WSH and P2TR?
- What happens to privacy when script paths are revealed?
- What does P2MR require from wallets and address standards?
- How would future PQ signature opcodes or leaves fit into this design?

---

### [BIP361 — Post Quantum Migration and Legacy Signature Sunset](https://bips.dev/361/)

**Type:** Bitcoin Improvement Proposal  
**Labels:** `draft-bip`, `informational`, `migration`, `controversial`, `legacy-signatures`  
**Difficulty:** Advanced  
**Bitcoin relevance:** BIP361 addresses what happens after a PQ output type exists.  
**Key takeaway:** This is not only a cryptographic proposal; it is about incentives, timing, legacy outputs, economic security, and social consensus.

**What it tries to solve:**

- Incentivizing migration away from quantum-vulnerable address types.
- Reducing incentives for a future quantum attacker to target old UTXOs.
- Providing a structured phase-out framework for legacy ECDSA/Schnorr signatures.

**Important caveats:**

- Requires a future PQ signature/output BIP.
- Raises major questions about property rights, lost coins, dormant coins, and Bitcoin’s social contract.
- Should be read alongside critiques and alternatives.

**Questions to ask while reading:**

- Is forced migration compatible with Bitcoin’s values?
- What happens to users who cannot migrate?
- What happens to lost coins?
- How would exchanges, custodians, and hardware wallets coordinate?
- Is there a less intrusive way to reduce attack incentives?
- How do migration timelines relate to uncertainty around quantum hardware?

---

## Live Delving Bitcoin discussions

### [Delving Bitcoin — post-quantum tag](https://delvingbitcoin.org/tag/post-quantum)

**Type:** Forum tag / live discussion index  
**Labels:** `discussion`, `live`, `needs-review`  
**Difficulty:** Mixed  
**Bitcoin relevance:** Provides the most current map of active post-quantum Bitcoin discussion.  
**Key takeaway:** Use this as a feed, not as a source of consensus. Track ideas, questions, objections, and references.

**Curation practice:**

- Record the date you last checked each thread.
- Capture the main claim in your own words.
- Record serious objections.
- Record whether code exists.
- Record whether a BIP exists.
- Mark stale or superseded threads.

---

### [Witness Version 3: ML-DSA-65 Post-Quantum Key-Path Spending](https://delvingbitcoin.org/t/bip-draft-witness-version-3-ml-dsa-65-post-quantum-key-path-spending/2422)

**Type:** Delving Bitcoin proposal discussion  
**Labels:** `discussion`, `implementation-experiment`, `ml-dsa`, `new-witness-version`, `needs-review`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Concrete discussion of using NIST FIPS 204 ML-DSA-65 in a new witness version.  
**Key takeaway:** This is useful because it makes the tradeoffs concrete: witness size, public-key size, validation cost, sighash design, dependency risk, and deployment path.

**Questions to ask:**

- Why choose ML-DSA-65 instead of ML-DSA-44, SLH-DSA, Falcon/FN-DSA, or hash-based schemes?
- Should PQ verification live in a new witness version or Tapscript?
- How should Bitcoin account for validation cost?
- What are the risks of liboqs or any external implementation in consensus-critical code?
- How would wallets estimate fees and manage large witnesses?

---

### [P2WOTS — Post Quantum UTXO Winternitz Signatures](https://delvingbitcoin.org/t/p2wots-post-quantum-utxo-winternitz-signatures/2530)

**Type:** Delving Bitcoin proposal discussion  
**Labels:** `discussion`, `wots`, `hash-based`, `one-time-signature`, `needs-review`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Explores Winternitz-style signatures in a Bitcoin UTXO setting.  
**Key takeaway:** Very useful for understanding why one-time signatures are tempting and dangerous. Bitcoin wallets often need retries, RBF/CPFP strategies, multi-user signing, and protection against address reuse.

**Questions to ask:**

- What happens if a WOTS key signs more than once?
- How does the proposal handle address reuse?
- How does it handle fee bumping?
- Can it support multisig or multi-user transactions?
- What are the wallet footguns?
- Is the witness version already claimed by another proposal?

---

### [Post-Quantum Migrations via Commit-then-Reveal](https://delvingbitcoin.org/t/post-quantum-migrations-via-commit-then-reveal-ala-bip16-an-alternative-to-sunsetting/2467)

**Type:** Delving Bitcoin proposal discussion  
**Labels:** `discussion`, `migration`, `commit-reveal`, `short-exposure`, `mempool-race`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Addresses the problem of moving legacy coins when the public key must be revealed during spend.  
**Key takeaway:** Migration is not just “send funds to a PQ address.” For some outputs, the act of migration may reveal the public key and create a race.

**Questions to ask:**

- What exactly is committed before reveal?
- How long must the commitment be buried?
- What attacker mining/relay assumptions are used?
- Does the scheme work for P2PK, P2PKH, P2WPKH, P2SH, P2WSH, and P2TR?
- How does it compare with BIP361-style sunsetting?
- What are the UX costs?

---

### [QCAP — A Bitcoin-Native Quantum Canary Alert](https://delvingbitcoin.org/t/qcap-a-bitcoin-native-quantum-canary-alert/2498)

**Type:** Delving Bitcoin proposal discussion  
**Labels:** `discussion`, `quantum-canary`, `monitoring`, `research`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Proposes a native on-chain canary-style signal for quantum progress against weaker curve assumptions.  
**Key takeaway:** PQ readiness may require objective signals, not just hardware press releases. Canary designs try to create a publicly verifiable alert mechanism.

**Questions to ask:**

- What exactly would the canary prove?
- Can it distinguish classical compromise from quantum compromise?
- Who constructs the canary and what trust assumptions exist?
- What curve or challenge hardness is used?
- What false-positive or false-negative risks exist?
- What action should the Bitcoin ecosystem take if the canary is spent?

---

### [SHRINCS — 324-byte stateful post-quantum signatures with static backups](https://delvingbitcoin.org/t/shrincs-324-byte-stateful-post-quantum-signatures-with-static-backups/2158)

**Type:** Delving Bitcoin proposal discussion  
**Labels:** `discussion`, `stateful-signatures`, `hash-based`, `needs-review`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Explores compact stateful PQ signatures.  
**Key takeaway:** Compactness is attractive for Bitcoin, but statefulness must be evaluated against wallet backups, signing retries, and real-world operational failure.

---

### [Compact Isogeny PQC can replace HD wallets, key-tweaking, silent payments](https://delvingbitcoin.org/t/compact-isogeny-pqc-can-replace-hd-wallets-key-tweaking-silent-payments/2324)

**Type:** Delving Bitcoin discussion  
**Labels:** `discussion`, `isogeny`, `wallets`, `needs-review`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Discusses whether compact isogeny-based PQC could support wallet features that lattice or hash-based schemes struggle with.  
**Key takeaway:** Useful as a speculative research direction. Treat carefully because isogeny-based cryptography has had major scheme failures in the broader PQC landscape.

---

### [Post-Quantum HD-Wallets, Silent Payments, Key Aggregation, and Threshold Signatures](https://delvingbitcoin.org/t/post-quantum-hd-wallets-silent-payments-key-aggregation-and-threshold-signatures/1854)

**Type:** Delving Bitcoin discussion  
**Labels:** `discussion`, `wallets`, `lattices`, `silent-payments`, `threshold`, `research`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Explores whether PQ primitives can eventually support current wallet functionality such as HD derivation, silent payments, key aggregation, and threshold signing.  
**Key takeaway:** A Bitcoin PQ migration must preserve or replace wallet functionality, not merely add a new single-signature output.

---

## bitcoin-dev mailing-list discussions

### [The limitations of cryptographic agility in Bitcoin](https://groups.google.com/g/bitcoindev/c/O6l3GUvyO7A)

**Type:** bitcoin-dev mailing-list discussion  
**Labels:** `discussion`, `cryptographic-agility`, `social-consensus`, `economics`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Discusses why adding a new opt-in cryptographic primitive may not fully remove Bitcoin’s collective dependence on older cryptographic assumptions.  
**Key takeaway:** Bitcoin is a shared monetary system. Even users who migrate may still be economically affected by large amounts of non-migrated vulnerable coins.

**Questions to ask:**

- What does “collective security assumption” mean?
- Does opt-in migration solve systemic risk?
- How do fungibility and market confidence affect cryptographic migration?
- What does this imply for “just add a PQ address type” proposals?

---

### [Taproot is post-quantum secure when restricted to script-path spends](https://groups.google.com/g/bitcoindev/c/ydE5u5C0xVc)

**Type:** bitcoin-dev mailing-list discussion  
**Labels:** `discussion`, `taproot`, `commitment-security`, `script-path`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Discusses the security of Taproot as a commitment scheme when restricted to script-path spends, based on Tim Ruffing’s paper.  
**Key takeaway:** This discussion is important for proposals that add PQ signatures to script paths or remove/disallow key-path spends.

---

### [Post-Quantum commit/reveal Fawkescoin variant as a soft fork](https://groups.google.com/g/bitcoindev/c/LpWOcXMcvk8)

**Type:** bitcoin-dev mailing-list discussion  
**Labels:** `discussion`, `commit-reveal`, `migration`, `soft-fork`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Explores commit/reveal-style migration approaches that defer some decisions until a quantum threat materializes.  
**Key takeaway:** Good counterpart to sunsetting approaches. Compare with BIP361 and Delving Bitcoin commit-then-reveal discussions.

---

### [Trivial QC signatures with clean upgrade path](https://groups.google.com/g/bitcoindev/c/8O857bRSVV8/m/8nr6I5NIAwAJ)

**Type:** bitcoin-dev mailing-list discussion  
**Labels:** `discussion`, `hash-based`, `opcodes`, `upgrade-path`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Discusses possible hash-based or script-based PQ signature paths.  
**Key takeaway:** Useful for understanding why some developers consider conservative hash-based signatures as an initial fallback despite size and UX costs.

---

### [Against Allowing Quantum Recovery of Bitcoin](https://groups.google.com/g/bitcoindev/c/uUK6py0Yjq0/m/pHihbvlwAQAJ)

**Type:** bitcoin-dev mailing-list discussion  
**Labels:** `discussion`, `ethics`, `lost-coins`, `migration`, `controversial`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Engages the ethical and social question of what should happen to vulnerable coins in a quantum scenario.  
**Key takeaway:** PQ migration design is entangled with property rights, theft prevention, lost coins, and Bitcoin’s social contract.

---

### [Hashed keys are actually fully quantum secure](https://groups.google.com/g/bitcoindev/c/jr1QO95k6Uc)

**Type:** bitcoin-dev mailing-list discussion  
**Labels:** `discussion`, `hashed-keys`, `taproot`, `migration`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Discusses hashed public keys, Taproot, and what can remain protected before public keys are revealed.  
**Key takeaway:** Useful but subtle. Do not summarize casually; distinguish hidden keys, revealed keys, descriptors, address reuse, and key-path spends.

---

## Additional PQ signature integration discussions

### [SHRIMPS](https://delvingbitcoin.org/t/shrimps-2-5-kb-post-quantum-signatures-across-multiple-stateful-devices/2355/1)

**Type:** Delving Bitcoin discussion  
**Labels:** `discussion`, `hash-based`, `stateful-signatures`, `needs-review`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Improve backup and multi-device practicality.  
**Key takeaway:** The March 2026 discussion proposes a compact path whose security depends on a bounded number of device uses. Treat the reported size as parameter-specific.

**Questions to ask while reading:** How many device initializations and compact-path signatures are allowed? What happens when a device loses state? Compare the recovery cost with SHRINCS and SLH-DSA.

**Tracker reference:** PQ-06; [proposal tracker](../proposal-tracker.md).

---

### [OP_CHECKSHRINCS](https://blog.blockstream.com/op_checkshrincs-a-hash-based-signature-opcode-for-post-quantum-bitcoin)

**Type:** Author technical design article  
**Labels:** `discussion`, `implementation-experiment`, `hash-based`, `opcodes`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Potential native PQ authorization in script leaves.  
**Key takeaway:** The article explores an opcode and reports experimental work, not an active Bitcoin consensus rule. Follow the linked specification for later revisions.

**Questions to ask while reading:** How are the signature message and encoding specified? What validation costs and stack limits must change? Can a wallet safely manage fallback signing?

**Tracker reference:** PQ-07; [proposal tracker](../proposal-tracker.md).

---

### [Handling PQ signatures on the stack](https://delvingbitcoin.org/t/handling-pq-signatures-on-the-stack/3128)

**Type:** Delving Bitcoin discussion  
**Labels:** `discussion`, `bitcoin-core`, `needs-review`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Representation of large PQ signature data.  
**Key takeaway:** The November 27 thread is about safely representing larger Script elements. An interpreter optimization by itself does not activate new consensus rules.

**Questions to ask while reading:** How should memory consumption be measured? What constraints prevent adversarial copying and oversized allocations? Which changes are consensus and which are implementation-only?

**Tracker reference:** PQ-08; [proposal tracker](../proposal-tracker.md).

---

## Additional migration and legacy-recovery discussions

### [Hourglass V2](https://delvingbitcoin.org/t/hourglass-v2-update/2246)

**Type:** Draft design discussion  
**Labels:** `discussion`, `migration`, `controversial`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Limit pace of potentially disruptive spending.  
**Key takeaway:** The February 2026 v2 draft adds a net-value cap to the P2PK throughput restriction. The cap does not establish original ownership.

**Questions to ask while reading:** How does throttling affect ordinary owners, fees and waiting time? What happens before activation? Can it coexist with a recovery mechanism?

**Tracker reference:** PQ-10; [proposal tracker](../proposal-tracker.md).

---

### [BIP86 recovery via zk-STARK seed knowledge](https://groups.google.com/g/bitcoindev/c/Q06piCEJhkI)

**Type:** bitcoin-dev discussion with prototype references  
**Labels:** `discussion`, `implementation-experiment`, `recovery`, `needs-review`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Potential independent-secret recovery for covered HD wallets.  
**Key takeaway:** A proof of derivation from secret BIP32 material is one possible source of knowledge asymmetry; a prototype does not define a production rescue rule.

**Questions to ask while reading:** What is the exact proof statement? Which derivation paths and wallet types are covered? What binds the proof to the intended spend? What is disclosed?

**Tracker reference:** PQ-11; [proposal tracker](../proposal-tracker.md).

---

### [Post Quantum Recovery Framework](https://delvingbitcoin.org/t/post-quantum-recovery-framework/3099)

**Type:** Delving Bitcoin discussion  
**Labels:** `discussion`, `recovery`, `research`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Clarify what legacy rescue must establish.  
**Key takeaway:** The November 20 framework provides vocabulary for comparing recovery approaches. It is not a claim that a single rescue mechanism covers all legacy coins.

**Questions to ask while reading:** Which evidence was prepared before a compromise? Which independent secrets could exist without preparation? How are claims authenticated and bound to transactions?

**Tracker reference:** PQ-12; [proposal tracker](../proposal-tracker.md).

---

### [BIP361 + PACTs](https://delvingbitcoin.org/t/a-bitcoin-quantum-roadmap-combining-bip-361-and-pacts-to-preserve-ownership-rights/3122)

**Type:** Delving Bitcoin roadmap discussion  
**Labels:** `discussion`, `migration`, `controversial`, `recovery`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Preserve potential claims by owners who commit before compromise.  
**Key takeaway:** The November 25 thread considers integrating timestamped ownership commitments with restrictions and a maturation period. Its schedule is a proposal, not BIP361 consensus text.

**Questions to ask while reading:** How is a historical address-control claim authenticated? Who determines a registration cutoff? Can a late registrant be distinguished from an attacker?

**Tracker reference:** PQ-13; [proposal tracker](../proposal-tracker.md).

---

### [QSAVE](https://groups.google.com/g/bitcoindev/c/5G38b3JL9fA)

**Type:** bitcoin-dev proposal series  
**Labels:** `discussion`, `migration`, `recovery`, `controversial`  
**Difficulty:** Advanced  
**Bitcoin relevance:** Alternative treatment of unmigrated coins without a simple sunset.  
**Key takeaway:** QSAVE explores a recovery-output alternative to a simple sunset. See [Bitcoin Optech #430](https://bitcoinops.org/en/newsletters/2026/11/06/) for an overview, then evaluate the original draft material.

**Questions to ask while reading:** What second-factor evidence is accepted? How are recovery outputs enforced? What custody and governance assumptions apply when evidence is unavailable?

**Tracker reference:** PQ-14; [proposal tracker](../proposal-tracker.md).

---

## Research digests and source discovery

### [Bitcoin Optech — Quantum resistance](https://bitcoinops.org/en/topics/quantum-resistance/)

**Type:** Maintained technical topic index  
**Labels:** `discussion`, `live`  
**Difficulty:** Intermediate to advanced  
**Bitcoin relevance:** Useful starting point for following changes in Bitcoin PQ design proposals.  
**Key takeaway:** Follow the linked original sources and check their revisions. A newsletter or topic summary is secondary evidence, not consensus authority.

### [Bitcoin Optech Newsletter #430 — November 6, 2026](https://bitcoinops.org/en/newsletters/2026/11/06/)

**Type:** Technical newsletter  
**Labels:** `discussion`, `migration`, `needs-review`  
**Difficulty:** Intermediate to advanced  
**Bitcoin relevance:** Covers QSAVE and a separate proof-of-decay idea, among other topics.  
**Key takeaway:** Compare proposals according to their authorization evidence, spending constraints, legitimate-owner access and economic effects rather than treating all migration restrictions as equivalent.
