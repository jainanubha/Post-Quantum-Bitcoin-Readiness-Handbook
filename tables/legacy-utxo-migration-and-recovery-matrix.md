# Legacy UTXO migration and recovery matrix

## Scope

This matrix describes potential exposure and research directions, not eligibility for any deployed recovery scheme. It assumes the ability to identify the script family but not necessarily the owner's wallet construction or off-chain disclosures. Exposure means relevant public information is available to an attacker; a usable forgery can still require additional script or authorization data. Dormancy and lost-key status must not be inferred from age alone.

| Output or condition | Public information at creation | Additional disclosure risk | Ordinary migration requirement | Potential independent evidence |
| --- | --- | --- | --- | --- |
| P2PK | Public key | Already exposed | Valid legacy signature | Separate secret if one exists; no general method established |
| Bare multisig | Public keys and threshold script | Already exposed | Required legacy signatures | Independent evidence for authorized participants, policy-specific |
| Fresh P2PKH | HASH160 of public key | Public key revealed at spend or off-chain | Signature and public key | Unrevealed key before disclosure; compatible HD proof if available |
| Reused P2PKH | Key hash | Public key may already be known from prior spend | Signature and public key | Independent secret beyond recovered scalar |
| Fresh P2WPKH | HASH160 of public key | Witness or off-chain disclosure | Signature and public key | Hidden-key or HD evidence, subject to threat model |
| Reused P2WPKH | Key hash | Public key may already be exposed | Signature and public key | Independent secret, if retained |
| P2SH | HASH160 of redeem script | Script and relevant keys may be disclosed | Satisfy redeem script and existing rules | Script or independent secrets, policy-dependent |
| P2WSH | SHA256 of witness script | Witness script and relevant keys may be disclosed | Satisfy witness script and existing rules | Script or independent secrets, policy-dependent |
| P2TR | X-only EC output key | Direct key path is quantum-vulnerable irrespective of intended script-only usage | Key path or valid script path | Compatible HD derivation plus tweak relation, or other independent evidence |
| Dormant but accessible | Depends on script type | Depends on prior disclosures | Existing authorized holder must act | Wallet-specific evidence |
| Standalone imported key | Depends on script type | Depends on prior disclosures | Ordinary authorization if key retained | No HD proof can be presumed |
| Lost or inaccessible material | Depends on script type | Depends on prior disclosures | May be impossible | Unknown; recovery not guaranteed |

## Suggested inventory schema

| Field | Example values / notes |
| --- | --- |
| Snapshot | Block height and date |
| Script family | P2TR, P2WPKH, etc. |
| Value | satoshis; aggregate separately from counts |
| Known public-key exposure | Direct / previous disclosure / no known disclosure / unknown |
| Authorization | Single-key / multiparty / script-dependent / unknown |
| Owner accessibility | Available / inaccessible / unknown; not generally inferable on-chain |
| Potential independent evidence | HD material / hidden key or script / prior commitment / other / unknown |
| Estimated migration weight | Model and assumptions; exclude unimplemented rescue costs |
| Recovery status | Research candidate only until actual rules exist |

Do not store private keys, seeds or other sensitive recovery witnesses in a public inventory. An unknown classification is not an assurance of safety. BIP361 specifically notes the difficulty of rescue for early P2PK outputs; an output type does not reveal whether the owner retains an independent secret.

## References

- [BIP361](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki)
- [BIP32 — Hierarchical Deterministic Wallets](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)
- [BIP341 — Taproot](https://github.com/bitcoin/bips/blob/master/bip-0341.mediawiki)
- [Post Quantum Recovery Framework — Delving Bitcoin, November 20, 2026](https://delvingbitcoin.org/t/post-quantum-recovery-framework/3099/1)
