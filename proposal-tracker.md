# Bitcoin Post-Quantum Proposal Tracker

This tracker records what a proposal intends to address, not what it has been proven to solve. A draft BIP, a working prototype, a research paper and a final NIST publication establish different kinds of evidence.

Related resources: [open research questions](research/open-questions.md), [proposal reading list](reading-list/bitcoin-post-quantum-proposals-and-discussions.md), [academic papers](reading-list/academic-papers.md), [source policy](SOURCE-POLICY.md).

## Status conventions and maintenance

| Label | Meaning |
| --- | --- |
| `draft-bip` | Numbered BIP whose official status is Draft. The type may be Specification or Informational. |
| `discussion` | Public design proposal or discussion, not a consensus decision. |
| `draft specification` | Proposed technical text, not necessarily assigned a BIP number. |
| `implementation-experiment` | Prototype or reference implementation reported; not independently validated here. |
| `research` | Cryptographic construction, preprint or research direction. |
| `draft recommendation` | Standards-body draft, not final. |
| `final report` | Published standards-body report; does not imply all discussed schemes are standardized. |


## Proposal index

### Formal BIPs

| ID | Proposal / source | Status | Main idea | Intended contribution | Limitations / dependencies |
| --- | --- | --- | --- | --- | --- |
| PQ-01 | [BIP360 — Pay-to-Merkle-Root](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki) | draft-bip | Commit directly to a script-tree root with witness v2; remove the elliptic-curve output key. | Conditional mitigation of long exposure for hidden leaf keys. | No PQ verifier, no retrofit of legacy coins; depth-zero script is not enforced. BIPs 340–342; activation; safe tree construction. |
| PQ-02 | [BIP361 — Post Quantum Migration and Legacy Signature Sunset](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki) | draft-bip (informational) | Schedule restrictions on vulnerable destinations and later tighten legacy authorization with rescue conditions. | Migration coordination and collective risk. | No complete eligibility predicate, rescue verifier or activation specification; access risk. Future PQ signature BIP; rescue design; deployment. |

### Signature integration

| ID | Proposal / source | Status | Main idea | Intended contribution | Limitations / dependencies |
| --- | --- | --- | --- | --- | --- |
| PQ-03 | [Witness v3 — ML-DSA-65](https://delvingbitcoin.org/t/bip-draft-witness-version-3-ml-dsa-65-post-quantum-key-path-spending/2422) | discussion; implementation-experiment | Dedicated witness version for ML-DSA-65 spending. | Candidate PQ transaction authorization. | Larger signatures and keys; resource accounting and consensus-safe implementation remain open. FIPS 204; exact sighash, encoding, validation and activation. |
| PQ-04 | [P2WOTS](https://delvingbitcoin.org/t/p2wots-post-quantum-utxo-winternitz-signatures/2530) | discussion; implementation-experiment | Explore Winternitz-based UTXO authorization and transaction-linked key use. | Potential compact hash-based authorization. | Multiple signatures, replacement, state and address lifecycle must be analyzed. One-time-key security; precise transaction binding and proposed witness rules. |
| PQ-05 | [SHRINCS](https://delvingbitcoin.org/t/shrincs-324-byte-stateful-post-quantum-signatures-with-static-backups/2158) | research | Combine efficient stateful hash signatures with stateless fallback under a commitment. | Reduce common-case signature overhead and allow static seed restoration. | Rollback or undetected state reuse; larger recovery signatures. Bounded usage; reliable signer state; fallback parameters; Bitcoin integration. |
| PQ-06 | [SHRIMPS](https://delvingbitcoin.org/t/shrimps-2-5-kb-post-quantum-signatures-across-multiple-stateful-devices/2355/1) | research | Use a bounded-use compact hash signature path across independently initialized devices, plus a fallback. | Improve backup and multi-device practicality. | Device resets consume the signing budget; larger fallback; no consensus integration. Bound on device initializations and per-key signing use. |
| PQ-07 | [OP_CHECKSHRINCS](https://blog.blockstream.com/op_checkshrincs-a-hash-based-signature-opcode-for-post-quantum-bitcoin) | discussion; implementation-experiment | Specify a Script verification path for a hash-based signature construction. | Potential native PQ authorization in script leaves. | Algorithm research is distinct from safe opcode deployment; no live Bitcoin opcode. Reviewed encodings, sighash, stack limits, activation and signer safety. |
| PQ-08 | [Handling PQ signatures on the stack](https://delvingbitcoin.org/t/handling-pq-signatures-on-the-stack/3128) | discussion | Evaluate larger stack elements and interpreter memory handling. | Representation of large PQ signature data. | Does not choose a signature scheme or prove bounded validation costs. Script extension, memory/CPU budget, malformation handling. |

### Migration and recovery

| ID | Proposal / source | Status | Main idea | Intended contribution | Limitations / dependencies |
| --- | --- | --- | --- | --- | --- |
| PQ-09 | [Commit-then-reveal migration](https://delvingbitcoin.org/t/post-quantum-migrations-via-commit-then-reveal-ala-bip16-an-alternative-to-sunsetting/2467) | discussion | Anchor a commitment before revealing legacy authorization and the migration destination. | Mitigate certain short-exposure migration races. | Already exposed keys may allow competing claims; reorg and censorship assumptions matter. Secure binding, commitment ordering and delay, consensus rules. |
| PQ-10 | [Hourglass V2](https://delvingbitcoin.org/t/hourglass-v2-update/2246) | discussion; draft specification | Restrict specified P2PK spending throughput; v2 proposes one spend and at most one net BTC per block. | Limit pace of potentially disruptive spending. | Does not authenticate original owners; queues and fee competition also affect legitimate holders. Exact restriction, activation, interaction with migration and rescue. |
| PQ-11 | [BIP86 recovery via zk-STARK seed knowledge](https://groups.google.com/g/bitcoindev/c/Q06piCEJhkI) | discussion; implementation-experiment | Prove a BIP86 output is derived from BIP32 seed material without disclosing that material. | Potential independent-secret recovery for covered HD wallets. | Not a deployed verifier; excludes unrelated keys and compromised ancestor secrets; needs spend binding. BIP32/BIP86, secure proof system, precise public statement and consensus predicate. |
| PQ-12 | [Post Quantum Recovery Framework](https://delvingbitcoin.org/t/post-quantum-recovery-framework/3099) | discussion | Organize recovery mechanisms by preparation and additional knowledge available to owners. | Clarify what legacy rescue must establish. | Not a single finished mechanism or evidence of recovery coverage. Threat model, explicit evidence types and verification rules. |
| PQ-13 | [BIP361 + PACTs](https://delvingbitcoin.org/t/a-bitcoin-quantum-roadmap-combining-bip-361-and-pacts-to-preserve-ownership-rights/3122) | discussion | Combine legacy restrictions with matured timestamped address-control commitments and a registration cutoff. | Preserve potential claims by owners who commit before compromise. | Cutoff uncertainty; unregistered holders; privacy, proof and liveness requirements. Commitment format, recovery verifier, maturation rules and cutoff governance. |
| PQ-14 | [QSAVE](https://groups.google.com/g/bitcoindev/c/5G38b3JL9fA) | discussion; draft proposals | Require additional evidence or direct unmigrated funds to a recovery output under a proposed escrow framework. | Alternative treatment of unmigrated coins without a simple sunset. | Recovery custody, eligibility, governance and second-factor coverage require review. Defined PQ types, proof/binding verifier, escrow and deployment rules. |

### Wallet cryptography

| ID | Proposal / source | Status | Main idea | Intended contribution | Limitations / dependencies |
| --- | --- | --- | --- | --- | --- |
| PQ-15 | [PQ HD wallets, silent payments, aggregation and threshold signatures](https://delvingbitcoin.org/t/post-quantum-hd-wallets-silent-payments-key-aggregation-and-threshold-signatures/1854) | discussion; research | Explore lattice-based analogues of derivation, stealth addresses and joint signing. | Preserve wallet features beyond single-key authorization. | No drop-in replacement for BIP32, MuSig2 or BIP352 established. Scheme-specific security, derivation and interoperability. |
| PQ-16 | [Compact lattice threshold signatures](https://eprint.iacr.org/2025/872) | research | Develop threshold signatures with reduced on-chain overhead. | Potential distributed custody without revealing independent signatures. | Does not establish BIP340 or ML-DSA compatibility; communication and threat model matter. Reviewed scheme, key generation, signer protocol, consensus integration. |

### Quantum monitoring and governance

| ID | Proposal / source | Status | Main idea | Intended contribution | Limitations / dependencies |
| --- | --- | --- | --- | --- | --- |
| PQ-17 | [QCAP quantum canary](https://delvingbitcoin.org/t/qcap-a-bitcoin-native-quantum-canary-alert/2498) | discussion | Collaboratively construct a challenge linked to discrete-log capability on a weaker curve. | Potential public warning signal. | No reliable calendar forecast; classical solutions, hidden capability and setup assumptions complicate interpretation. Challenge security, incentive model, interpretation and response rules. |
| PQ-18 | [Limits of cryptographic agility](https://groups.google.com/g/bitcoindev/c/O6l3GUvyO7A) | discussion | Examine shared monetary exposure to cryptographic choices made by other holders. | Frame collective-security and adoption trade-offs. | Not an empirical valuation model or recommendation for a particular fork. Adoption scenarios and governance analysis. |

### Standards and supporting research

| ID | Proposal / source | Status | Main idea | Intended contribution | Limitations / dependencies |
| --- | --- | --- | --- | --- | --- |
| PQ-19 | [NIST SP 800-230](https://csrc.nist.gov/pubs/sp/800/230/ipd) | draft recommendation | Study additional SLH-DSA profiles for bounded numbers of signatures. | Possible smaller signatures in restricted-use applications. | Draft profiles not finalized; a single UTXO can be signed repeatedly. Enforceable signing limits; wallet state and recovery analysis. |
| PQ-20 | [NIST IR 8610](https://csrc.nist.gov/pubs/ir/8610/final) | final report | Record progress of additional PQ signature candidates into the next evaluation round. | Track alternative assumptions and design trade-offs. | Advancement is not standardization, Bitcoin adoption or deployment. Continue candidate review and cryptanalysis. |
| PQ-21 | [Hash-based Signature Schemes for Bitcoin](https://eprint.iacr.org/2025/2203) | research | Analyze hash-based schemes and Bitcoin-specific parameter selection. | Ground compact signature comparisons and usage assumptions. | No automatic consensus approval or proof that wallet state is safe. Parameter review, independent cryptanalysis and resource analysis. |
| PQ-22 | [Lattice-based Signature Schemes for Bitcoin](https://eprint.iacr.org/2026/1628) | research | Compare Dilithium, Falcon and Hawk in the Bitcoin setting. | Evaluate footprint, implementation and wallet trade-offs. | Not a deployment decision or a finalized FN-DSA specification. Consistent security models, implementations, exact spending formats. |

## Dependencies and threat coverage

| Design layer | Related IDs | Dependency / unresolved boundary |
| --- | --- | --- |
| Hidden output commitment | PQ-01 | Secure committed leaves and undisclosed keys; does not stop fast recovery from revealed classical keys. |
| PQ authorization | PQ-03–08, PQ-19–22 | Exact encodings, security assumptions, deterministic verification and resource accounting. |
| Migration policy | PQ-02, PQ-09, PQ-10, PQ-13, PQ-14 | Available secure destinations; activation and treatment of legacy funds. |
| Recovery | PQ-11–14 | Evidence that remains independent of recovered ECC scalars, sound proof verification and transaction binding. |
| Wallet functionality | PQ-05, PQ-06, PQ-15, PQ-16 | Derivation, backup, signing-state safety and distributed authorization. |
| Monitoring and coordination | PQ-17, PQ-18 | Threat evidence and governance; a canary is not proof of a universal attack timetable. |

The layers are not an implementation dependency graph or an endorsement of a combined protocol. In particular, a destination that hides keys is not necessarily a fully PQ-authorized destination, and a migration restriction is not itself a cryptographic primitive.
