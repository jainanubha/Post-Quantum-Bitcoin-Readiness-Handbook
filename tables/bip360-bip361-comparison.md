# BIP360 and BIP361 — comparison

## Specification basis

This table compares BIP360 v0.12.1 at its August 3, 2026 correction with the April 17, 2026 canonical BIP361 revision. Both are drafts, and neither is activated by publication. BIP361 is an informational framework; BIP360 is a specification for a proposed output type. The BIP361 project website uses different phase terminology and is not the specification basis here.

| Dimension | BIP360 — P2MR | BIP361 — migration and sunset |
| --- | --- | --- |
| Main purpose | Remove the exposed elliptic-curve output key from a script-tree output | Coordinate movement away from legacy vulnerable authorization |
| Scope | New native SegWit v2 output with a 32-byte script-tree root | Proposed restrictions on destinations and later legacy signature spends |
| Existing UTXOs | Not changed by the new output type | Proposed new restrictions may affect legacy spendability |
| Signature algorithm | Does not introduce PQ verification | Requires a future PQ signature BIP; does not select an algorithm |
| Long exposure | Removes the persistent Taproot-style output-key target, assuming safe leaf policies and disclosure practices | Seeks to reduce residual vulnerable value and restrict recovered-key authorization |
| Short exposure | Classical leaf keys may still be revealed on spend | Migration-time races remain possible; rescue or other mechanisms require specification |
| Commitment | Tagged Merkle root; no internal EC key or key path | No new output commitment format specified |
| Validation | Merkle proof and leaf validation; depth-zero commitment match succeeds without script execution | Destination eligibility and stronger legacy verification not yet fully specified |
| Timing | No activation by the draft itself | Phase A: 160,000 blocks after activation; Phase B: about two years after A |
| Recovery | Does not provide legacy-UTXO rescue | Envisages rescue in Phase B using independent evidence |
| Costs | Script path overhead and loss of key-path efficiency; smaller control block than equivalent P2TR script path | Migration demand, fee pressure, possible proof costs and access constraints |
| Governance | Opt-in new output type if activated | Potential network-wide changes to legacy spendability |
| Status | Draft, specification | Draft, informational |

## Dependency interpretation

A future PQ signature mechanism could use a suitable P2MR script path, or another output and validation construction. P2MR alone does not satisfy the requirement for quantum-resistant transaction authorization. BIP361 does not explicitly require BIP360. They are related research directions, not a fixed deployment bundle. A holder might migrate to a proposed new output only once its actual validation semantics are enforced. Even a valid new protected output does not make the legacy input used to fund it quantum-resistant.

## References

- [BIP360](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki)
- [BIP361](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki)
