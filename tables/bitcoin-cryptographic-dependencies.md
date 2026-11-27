# Bitcoin cryptographic dependencies

| Feature | Current dependency | PQ research question | Reference |
| --- | --- | --- | --- |
| Taproot | Schnorr and key tweaks | How to commit to scripts and authorize spends without vulnerable key paths? | [BIP 341](https://bips.dev/341/) |
| BIP32 | Elliptic-curve public derivation | How to support watch-only discovery and backups? | [BIP 32](https://bips.dev/32/) |
| MuSig2 | Schnorr joint signing | Can a PQ joint-signing protocol produce a compact signature? | [BIP 327](https://bips.dev/327/) |
| Threshold custody | Distributed signing | Which schemes offer secure threshold protocols and recovery? | [RFC 9591 (classical baseline)](https://www.rfc-editor.org/rfc/rfc9591.html) |
| Lightning | Repeated signatures, conditional payments, transport | What signing volumes and independent cryptographic changes are required? | [BOLTs](https://github.com/lightning/bolts) |
| PTLCs | Discrete-log-based adaptor constructions | What PQ conditional authorization is feasible? | [Adaptor signatures](https://bitcoinops.org/en/topics/adaptor-signatures/) |
| Silent Payments | Elliptic-curve shared secrets | How to preserve private discovery? | [BIP 352](https://bips.dev/352/) |
| DLCs | Adaptor signatures and oracle attestations | What alternative oracle and conditional spending design is needed? | [DLCs](https://bitcoinops.org/en/topics/discreet-log-contracts/) |
| PSBT | Existing key, signature and policy metadata | What algorithm-specific fields and signing semantics are needed? | [BIP 174](https://bips.dev/174/) |
| Message signing | Spending-condition verification | How should new output types be supported? | [BIP 322](https://bips.dev/322/) |
