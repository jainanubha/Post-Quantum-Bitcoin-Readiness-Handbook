# Open Research Questions

This register identifies research problems and expected evidence. The questions are handbook-generated analytical prompts, not claims that the cited authors endorse a particular solution. All questions start open; priority indicates the handbook's suggested research order, not consensus urgency.

See [proposal tracker](../proposal-tracker.md), [academic reading list](../reading-list/academic-papers.md), and [proposal reading list](../reading-list/bitcoin-post-quantum-proposals-and-discussions.md).

## Research index

| ID | Question area | Priority | Chapters | Status |
| --- | --- | --- | --- | --- |
| [RQ-01](#rq-01--quantum-attack-capability-and-exposure) | Quantum attack capability and exposure | High | 03–07 | Open |
| [RQ-02](#rq-02--exposure-inventory-and-measurement) | Exposure inventory and measurement | High | 06–08, 13 | Open |
| [RQ-03](#rq-03--security-categories-for-long-lived-utxos) | Security categories for long-lived UTXOs | High | 09–10 | Open |
| [RQ-04](#rq-04--selection-of-pq-signature-families) | Selection of PQ signature families | High | 09–11 | Open |
| [RQ-05](#rq-05--limited-use-signatures-and-state-safety) | Limited-use signatures and state safety | High | 09–10 | Open |
| [RQ-06](#rq-06--signature-aggregation-versus-batch-verification) | Signature aggregation versus batch verification | Medium | 10–11 | Open |
| [RQ-07](#rq-07--threshold-and-distributed-authorization) | Threshold and distributed authorization | High | 10 | Open |
| [RQ-08](#rq-08--large-witness-elements) | Large witness elements | High | 10, 12 | Open |
| [RQ-09](#rq-09--deterministic-validation-and-dos) | Deterministic validation and DoS | High | 10–11 | Open |
| [RQ-10](#rq-10--sighash-and-hybrid-authorization) | Sighash and hybrid authorization | High | 10, 12 | Open |
| [RQ-11](#rq-11--commitment-security-and-disclosure) | Commitment security and disclosure | High | 12 | Open |
| [RQ-12](#rq-12--depth-zero-and-upgradeable-leaves) | Depth-zero and upgradeable leaves | High | 12 | Open |
| [RQ-13](#rq-13--privacy-and-script-tree-structure) | Privacy and script-tree structure | Medium | 12 | Open |
| [RQ-14](#rq-14--concrete-spending-economics) | Concrete spending economics | Medium | 11–12 | Open |
| [RQ-15](#rq-15--phase-a-eligibility) | Phase A eligibility | High | 13 | Open |
| [RQ-16](#rq-16--phase-b-authorization) | Phase B authorization | High | 13 | Open |
| [RQ-17](#rq-17--knowledge-asymmetry-and-hd-recovery) | Knowledge asymmetry and HD recovery | High | 13 | Open |
| [RQ-18](#rq-18--prior-commitments-and-short-exposure) | Prior commitments and short exposure | High | 13 | Open |
| [RQ-19](#rq-19--unmigrated-funds-without-independent-evidence) | Unmigrated funds without independent evidence | High | 13 | Open |
| [RQ-20](#rq-20--capacity-under-constrained-block-space) | Capacity under constrained block space | High | 11, 13 | Open |
| [RQ-21](#rq-21--incentives-and-low-value-outputs) | Incentives and low-value outputs | High | 11, 13 | Open |
| [RQ-22](#rq-22--collective-risk-and-legacy-policy) | Collective risk and legacy policy | High | 13 | Open |
| [RQ-23](#rq-23--activation-and-canary-evidence) | Activation and canary evidence | Medium | 13 | Open |
| [RQ-24](#rq-24--wallet-and-custody-readiness) | Wallet and custody readiness | High | 10, 13–14 | Open |
| [RQ-25](#rq-25--cryptographic-agility-after-migration) | Cryptographic agility after migration | Medium | 09–10, 13 | Open |

## Threat modelling

### RQ-01 — Quantum attack capability and exposure

**Priority:** High  
**Status:** Open  
**Related chapters:** 03–07  
**Tracker IDs:** PQ-17, PQ-09

**Research question:** How do time-to-key distributions, parallelism and transaction confirmation affect long- and short-exposure risk?

**Investigation and evidence needed:** Compare explicit fault-tolerant resource assumptions and stochastic block/confirmation models; separate exposed and newly revealed keys.

**Expected research output:** A dated threat-model memo with sensitivity intervals, not a Q-day prediction.

**Starting references:** [QCAP quantum canary](https://delvingbitcoin.org/t/qcap-a-bitcoin-native-quantum-canary-alert/2498); [Commit-then-reveal migration](https://delvingbitcoin.org/t/post-quantum-migrations-via-commit-then-reveal-ala-bip16-an-alternative-to-sunsetting/2467).

### RQ-02 — Exposure inventory and measurement

**Priority:** High  
**Status:** Open  
**Related chapters:** 06–08, 13  
**Tracker IDs:** PQ-01, PQ-12

**Research question:** How can public-key exposure be classified without equating output count, BTC value or owner accessibility?

**Investigation and evidence needed:** Specify chain snapshot, script types, key reuse, historical disclosure, off-chain unknowns and denominators.

**Expected research output:** A documented classification method and uncertainty categories; no new blockchain scan required.

**Starting references:** [BIP360 — Pay-to-Merkle-Root](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki); [Post Quantum Recovery Framework](https://delvingbitcoin.org/t/post-quantum-recovery-framework/3099).

## Cryptographic primitives

### RQ-03 — Security categories for long-lived UTXOs

**Priority:** High  
**Status:** Open  
**Related chapters:** 09–10  
**Tracker IDs:** PQ-19, PQ-20, PQ-21, PQ-22

**Research question:** What security targets are appropriate across long lifetimes, multiple public keys and many signing queries?

**Investigation and evidence needed:** Compare NIST reference problems with concrete classical/quantum and multi-target assumptions.

**Expected research output:** A security-target rationale with explicit assumptions and no false equivalence to secp256k1.

**Starting references:** [NIST SP 800-230](https://csrc.nist.gov/pubs/sp/800/230/ipd); [NIST IR 8610](https://csrc.nist.gov/pubs/ir/8610/final); [Hash-based Signature Schemes for Bitcoin](https://eprint.iacr.org/2025/2203); [Lattice-based Signature Schemes for Bitcoin](https://eprint.iacr.org/2026/1628).

### RQ-04 — Selection of PQ signature families

**Priority:** High  
**Status:** Open  
**Related chapters:** 09–11  
**Tracker IDs:** PQ-03, PQ-21, PQ-22

**Research question:** Which combinations of security, size, maturity and composability meet Bitcoin requirements?

**Investigation and evidence needed:** Use common parameter/encoding definitions and separate cryptographic, implementation and consensus readiness.

**Expected research output:** A comparison matrix with gaps marked unknown, not a winner chosen from size alone.

**Starting references:** [Witness v3 — ML-DSA-65](https://delvingbitcoin.org/t/bip-draft-witness-version-3-ml-dsa-65-post-quantum-key-path-spending/2422); [Hash-based Signature Schemes for Bitcoin](https://eprint.iacr.org/2025/2203); [Lattice-based Signature Schemes for Bitcoin](https://eprint.iacr.org/2026/1628).

### RQ-05 — Limited-use signatures and state safety

**Priority:** High  
**Status:** Open  
**Related chapters:** 09–10  
**Tracker IDs:** PQ-04, PQ-05, PQ-06, PQ-19

**Research question:** Can bounded signing remain safe under replacement, aborted signing, rollback and restored devices?

**Investigation and evidence needed:** Model key state, signing attempts, device resets and fallback paths; define a maximum safe signing budget.

**Expected research output:** A wallet lifecycle model with failure cases and security assumptions.

**Starting references:** [P2WOTS](https://delvingbitcoin.org/t/p2wots-post-quantum-utxo-winternitz-signatures/2530); [SHRINCS](https://delvingbitcoin.org/t/shrincs-324-byte-stateful-post-quantum-signatures-with-static-backups/2158); [SHRIMPS](https://delvingbitcoin.org/t/shrimps-2-5-kb-post-quantum-signatures-across-multiple-stateful-devices/2355/1); [NIST SP 800-230](https://csrc.nist.gov/pubs/sp/800/230/ipd).

### RQ-06 — Signature aggregation versus batch verification

**Priority:** Medium  
**Status:** Open  
**Related chapters:** 10–11  
**Tracker IDs:** PQ-15, PQ-16

**Research question:** Can aggregation reduce on-chain bytes without imposing unacceptable verification or coordination cost?

**Investigation and evidence needed:** Distinguish aggregate signatures, batch verification and joint signing; compare exact data commitments.

**Expected research output:** A scheme-specific feasibility survey with size and resource metrics.

**Starting references:** [PQ HD wallets, silent payments, aggregation and threshold signatures](https://delvingbitcoin.org/t/post-quantum-hd-wallets-silent-payments-key-aggregation-and-threshold-signatures/1854); [Compact lattice threshold signatures](https://eprint.iacr.org/2025/872).

### RQ-07 — Threshold and distributed authorization

**Priority:** High  
**Status:** Open  
**Related chapters:** 10  
**Tracker IDs:** PQ-15, PQ-16

**Research question:** Can PQ threshold schemes preserve custody security and privacy with manageable signer communication?

**Investigation and evidence needed:** Compare DKG, corruption models, aborts, malicious signers, share recovery and verifier compatibility.

**Expected research output:** A threshold-protocol requirements matrix and open security assumptions.

**Starting references:** [PQ HD wallets, silent payments, aggregation and threshold signatures](https://delvingbitcoin.org/t/post-quantum-hd-wallets-silent-payments-key-aggregation-and-threshold-signatures/1854); [Compact lattice threshold signatures](https://eprint.iacr.org/2025/872).

## Consensus integration

### RQ-08 — Large witness elements

**Priority:** High  
**Status:** Open  
**Related chapters:** 10, 12  
**Tracker IDs:** PQ-03, PQ-07, PQ-08

**Research question:** How can new Script rules safely represent signatures and keys exceeding existing stack limits?

**Investigation and evidence needed:** Analyze candidate opcodes, leaf/witness versions, parsing, memory copying and adversarial allocations.

**Expected research output:** An integration-options memo and resource-limit requirements.

**Starting references:** [Witness v3 — ML-DSA-65](https://delvingbitcoin.org/t/bip-draft-witness-version-3-ml-dsa-65-post-quantum-key-path-spending/2422); [OP_CHECKSHRINCS](https://blog.blockstream.com/op_checkshrincs-a-hash-based-signature-opcode-for-post-quantum-bitcoin); [Handling PQ signatures on the stack](https://delvingbitcoin.org/t/handling-pq-signatures-on-the-stack/3128).

### RQ-09 — Deterministic validation and DoS

**Priority:** High  
**Status:** Open  
**Related chapters:** 10–11  
**Tracker IDs:** PQ-03, PQ-07, PQ-08

**Research question:** How should resource costs be bounded for valid and malformed PQ signatures?

**Investigation and evidence needed:** Specify canonical parsing, worst-case execution, memory limits and signature-budget interaction.

**Expected research output:** A validation checklist and benchmark methodology; measurements deferred.

**Starting references:** [Witness v3 — ML-DSA-65](https://delvingbitcoin.org/t/bip-draft-witness-version-3-ml-dsa-65-post-quantum-key-path-spending/2422); [OP_CHECKSHRINCS](https://blog.blockstream.com/op_checkshrincs-a-hash-based-signature-opcode-for-post-quantum-bitcoin); [Handling PQ signatures on the stack](https://delvingbitcoin.org/t/handling-pq-signatures-on-the-stack/3128).

### RQ-10 — Sighash and hybrid authorization

**Priority:** High  
**Status:** Open  
**Related chapters:** 10, 12  
**Tracker IDs:** PQ-03, PQ-07, PQ-01

**Research question:** How can new signatures bind transaction semantics and resist cross-protocol replay or downgrade?

**Investigation and evidence needed:** Compare message construction, algorithm identification, AND/OR branches, domain separation and output commitments.

**Expected research output:** A threat-based comparison of explicit authorization policies.

**Starting references:** [Witness v3 — ML-DSA-65](https://delvingbitcoin.org/t/bip-draft-witness-version-3-ml-dsa-65-post-quantum-key-path-spending/2422); [OP_CHECKSHRINCS](https://blog.blockstream.com/op_checkshrincs-a-hash-based-signature-opcode-for-post-quantum-bitcoin); [BIP360 — Pay-to-Merkle-Root](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki).

## P2MR

### RQ-11 — Commitment security and disclosure

**Priority:** High  
**Status:** Open  
**Related chapters:** 12  
**Tracker IDs:** PQ-01

**Research question:** Under what assumptions does P2MR hide a useful classical signing key until spend?

**Investigation and evidence needed:** Model public descriptors, reused keys, known leaves, low-entropy scripts and quantum preimage/collision assumptions.

**Expected research output:** A threat matrix separating commitment binding from hiding and authorization.

**Starting references:** [BIP360 — Pay-to-Merkle-Root](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki).

### RQ-12 — Depth-zero and upgradeable leaves

**Priority:** High  
**Status:** Open  
**Related chapters:** 12  
**Tracker IDs:** PQ-01, PQ-07

**Research question:** What wallet safeguards are necessary to avoid relying on unenforced leaves or unactivated upgrade semantics?

**Investigation and evidence needed:** Review the pinned draft, depth-zero rule, leaf versions and OP_SUCCESSx behavior; identify invalid security expectations.

**Expected research output:** A policy safety checklist and specification-review questions.

**Starting references:** [BIP360 — Pay-to-Merkle-Root](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki); [OP_CHECKSHRINCS](https://blog.blockstream.com/op_checkshrincs-a-hash-based-signature-opcode-for-post-quantum-bitcoin).

### RQ-13 — Privacy and script-tree structure

**Priority:** Medium  
**Status:** Open  
**Related chapters:** 12  
**Tracker IDs:** PQ-01

**Research question:** How does revealed leaf depth and policy metadata affect privacy relative to P2TR and P2WSH?

**Investigation and evidence needed:** Compare representative trees and known policy distributions; account for missing key-path indistinguishability.

**Expected research output:** A qualitative leakage model with explicit assumptions.

**Starting references:** [BIP360 — Pay-to-Merkle-Root](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki).

### RQ-14 — Concrete spending economics

**Priority:** Medium  
**Status:** Open  
**Related chapters:** 11–12  
**Tracker IDs:** PQ-01, PQ-03, PQ-07

**Research question:** How do tree depth, CompactSize boundaries and future PQ leaf encodings change weight?

**Investigation and evidence needed:** Compare complete serialized witnesses and outputs under equivalent policies; separate weight from verification.

**Expected research output:** Worked templates and peer-reviewable arithmetic.

**Starting references:** [BIP360 — Pay-to-Merkle-Root](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki); [Witness v3 — ML-DSA-65](https://delvingbitcoin.org/t/bip-draft-witness-version-3-ml-dsa-65-post-quantum-key-path-spending/2422); [OP_CHECKSHRINCS](https://blog.blockstream.com/op_checkshrincs-a-hash-based-signature-opcode-for-post-quantum-bitcoin).

## Migration and recovery

### RQ-15 — Phase A eligibility

**Priority:** High  
**Status:** Open  
**Related chapters:** 13  
**Tracker IDs:** PQ-01, PQ-02, PQ-14

**Research question:** How could consensus define an eligible PQ destination when the committed spending policy is hidden?

**Investigation and evidence needed:** Evaluate exact script/output recognition, upgrade semantics, weaker alternate branches and grandfathering.

**Expected research output:** Candidate rule requirements and compatibility analysis; no assumed finalized predicate.

**Starting references:** [BIP360 — Pay-to-Merkle-Root](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki); [BIP361 — Post Quantum Migration and Legacy Signature Sunset](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki); [QSAVE](https://groups.google.com/g/bitcoindev/c/5G38b3JL9fA).

### RQ-16 — Phase B authorization

**Priority:** High  
**Status:** Open  
**Related chapters:** 13  
**Tracker IDs:** PQ-02, PQ-11, PQ-12, PQ-14

**Research question:** What additional predicate could reject quantum-derived legacy keys while preserving valid recovery claims?

**Investigation and evidence needed:** Compare rescue proof statements, transaction binding, replay protection, policy dependencies and failure conditions.

**Expected research output:** A formal requirements document and explicit remaining consensus questions.

**Starting references:** [BIP361 — Post Quantum Migration and Legacy Signature Sunset](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki); [BIP86 recovery via zk-STARK seed knowledge](https://groups.google.com/g/bitcoindev/c/Q06piCEJhkI); [Post Quantum Recovery Framework](https://delvingbitcoin.org/t/post-quantum-recovery-framework/3099); [QSAVE](https://groups.google.com/g/bitcoindev/c/5G38b3JL9fA).

### RQ-17 — Knowledge asymmetry and HD recovery

**Priority:** High  
**Status:** Open  
**Related chapters:** 13  
**Tracker IDs:** PQ-11, PQ-12

**Research question:** Which independently held secrets remain unavailable to an attacker who recovers a child ECC scalar?

**Investigation and evidence needed:** Analyze hardened versus non-hardened derivation, disclosed xpubs/chain codes, seed entropy, imported keys and backup availability.

**Expected research output:** A recovery-eligibility matrix; do not infer actual coverage from address type.

**Starting references:** [BIP86 recovery via zk-STARK seed knowledge](https://groups.google.com/g/bitcoindev/c/Q06piCEJhkI); [Post Quantum Recovery Framework](https://delvingbitcoin.org/t/post-quantum-recovery-framework/3099).

### RQ-18 — Prior commitments and short exposure

**Priority:** High  
**Status:** Open  
**Related chapters:** 13  
**Tracker IDs:** PQ-09, PQ-13, PQ-14

**Research question:** When can a prior commitment or timestamped ownership claim reduce a migration-time attack?

**Investigation and evidence needed:** Evaluate timing, competing claims, maturation/cutoff rules, censorship, reorgs and proof binding.

**Expected research output:** A comparative adversarial model for prepared and unprepared users.

**Starting references:** [Commit-then-reveal migration](https://delvingbitcoin.org/t/post-quantum-migrations-via-commit-then-reveal-ala-bip16-an-alternative-to-sunsetting/2467); [BIP361 + PACTs](https://delvingbitcoin.org/t/a-bitcoin-quantum-roadmap-combining-bip-361-and-pacts-to-preserve-ownership-rights/3122); [QSAVE](https://groups.google.com/g/bitcoindev/c/5G38b3JL9fA).

### RQ-19 — Unmigrated funds without independent evidence

**Priority:** High  
**Status:** Open  
**Related chapters:** 13  
**Tracker IDs:** PQ-02, PQ-10, PQ-12, PQ-14

**Research question:** What policies could address vulnerable early or imported keys when no additional ownership evidence is known?

**Investigation and evidence needed:** Separate dormant, lost and accessible coins; compare unrestricted, throttled, rescue and escrow outcomes.

**Expected research output:** A neutral trade-off assessment including legitimate access and theft risk.

**Starting references:** [BIP361 — Post Quantum Migration and Legacy Signature Sunset](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki); [Hourglass V2](https://delvingbitcoin.org/t/hourglass-v2-update/2246); [Post Quantum Recovery Framework](https://delvingbitcoin.org/t/post-quantum-recovery-framework/3099); [QSAVE](https://groups.google.com/g/bitcoindev/c/5G38b3JL9fA).

## Migration economics

### RQ-20 — Capacity under constrained block space

**Priority:** High  
**Status:** Open  
**Related chapters:** 11, 13  
**Tracker IDs:** PQ-02, PQ-09, PQ-10

**Research question:** What block-space budget is needed to migrate different UTXO populations while normal payments continue?

**Investigation and evidence needed:** Use explicit input/output templates, batching, weight budget, participation and fee-rate scenarios; do not extrapolate synthetic counts to chain inventory.

**Expected research output:** A sensitivity table and data requirements for later empirical analysis.

**Starting references:** [BIP361 — Post Quantum Migration and Legacy Signature Sunset](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki); [Commit-then-reveal migration](https://delvingbitcoin.org/t/post-quantum-migrations-via-commit-then-reveal-ala-bip16-an-alternative-to-sunsetting/2467); [Hourglass V2](https://delvingbitcoin.org/t/hourglass-v2-update/2246).

### RQ-21 — Incentives and low-value outputs

**Priority:** High  
**Status:** Open  
**Related chapters:** 11, 13  
**Tracker IDs:** PQ-02, PQ-10, PQ-18

**Research question:** How do deadline pressure, fees and consolidation affect participation and spendability?

**Investigation and evidence needed:** Compare owner-controlled batching, dust/uneconomical outputs, liquidity and heterogeneous adoption; distinguish cost of migration from later PQ spending.

**Expected research output:** Scenario analysis with assumptions and distributional effects.

**Starting references:** [BIP361 — Post Quantum Migration and Legacy Signature Sunset](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki); [Hourglass V2](https://delvingbitcoin.org/t/hourglass-v2-update/2246); [Limits of cryptographic agility](https://groups.google.com/g/bitcoindev/c/O6l3GUvyO7A).

## Governance

### RQ-22 — Collective risk and legacy policy

**Priority:** High  
**Status:** Open  
**Related chapters:** 13  
**Tracker IDs:** PQ-02, PQ-10, PQ-13, PQ-14, PQ-18

**Research question:** How should theft prevention, legitimate access and individual choice be weighed under uncertainty?

**Investigation and evidence needed:** Compare competing views and assumptions; analyze effects on owners, miners, exchanges and users; avoid assuming dormant funds are lost.

**Expected research output:** Balanced decision matrix with disputed premises clearly attributed.

**Starting references:** [BIP361 — Post Quantum Migration and Legacy Signature Sunset](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki); [Hourglass V2](https://delvingbitcoin.org/t/hourglass-v2-update/2246); [BIP361 + PACTs](https://delvingbitcoin.org/t/a-bitcoin-quantum-roadmap-combining-bip-361-and-pacts-to-preserve-ownership-rights/3122); [QSAVE](https://groups.google.com/g/bitcoindev/c/5G38b3JL9fA); [Limits of cryptographic agility](https://groups.google.com/g/bitcoindev/c/O6l3GUvyO7A).

### RQ-23 — Activation and canary evidence

**Priority:** Medium  
**Status:** Open  
**Related chapters:** 13  
**Tracker IDs:** PQ-02, PQ-17

**Research question:** What evidence could justify changing migration rules, and how should activation be coordinated?

**Investigation and evidence needed:** Study public warning signals, false positives, hidden quantum capability, node adoption and non-upgraded behavior.

**Expected research output:** An activation-readiness research checklist, not a recommended activation date.

**Starting references:** [BIP361 — Post Quantum Migration and Legacy Signature Sunset](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki); [QCAP quantum canary](https://delvingbitcoin.org/t/qcap-a-bitcoin-native-quantum-canary-alert/2498).

## Wallet and ecosystem

### RQ-24 — Wallet and custody readiness

**Priority:** High  
**Status:** Open  
**Related chapters:** 10, 13–14  
**Tracker IDs:** PQ-05, PQ-06, PQ-11, PQ-15, PQ-16

**Research question:** Which dependencies must change for wallet backup, descriptor import, PSBT, signing devices and distributed custody?

**Investigation and evidence needed:** Map classical derivation, signing, recovery and interoperability to possible PQ replacements; include Lightning and exchange deposit workflows.

**Expected research output:** Requirements inventory for December, no implementation.

**Starting references:** [SHRINCS](https://delvingbitcoin.org/t/shrincs-324-byte-stateful-post-quantum-signatures-with-static-backups/2158); [SHRIMPS](https://delvingbitcoin.org/t/shrimps-2-5-kb-post-quantum-signatures-across-multiple-stateful-devices/2355/1); [BIP86 recovery via zk-STARK seed knowledge](https://groups.google.com/g/bitcoindev/c/Q06piCEJhkI); [PQ HD wallets, silent payments, aggregation and threshold signatures](https://delvingbitcoin.org/t/post-quantum-hd-wallets-silent-payments-key-aggregation-and-threshold-signatures/1854); [Compact lattice threshold signatures](https://eprint.iacr.org/2025/872).

### RQ-25 — Cryptographic agility after migration

**Priority:** Medium  
**Status:** Open  
**Related chapters:** 09–10, 13  
**Tracker IDs:** PQ-02, PQ-18, PQ-20

**Research question:** How can Bitcoin respond to later cryptanalysis without creating unsafe fallbacks or new stranded UTXOs?

**Investigation and evidence needed:** Examine algorithm versioning, multiple assumptions, immutable commitments and migration incentives.

**Expected research output:** A lifecycle and upgradeability analysis tied to explicit spending policies.

**Starting references:** [BIP361 — Post Quantum Migration and Legacy Signature Sunset](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki); [Limits of cryptographic agility](https://groups.google.com/g/bitcoindev/c/O6l3GUvyO7A); [NIST IR 8610](https://csrc.nist.gov/pubs/ir/8610/final).
