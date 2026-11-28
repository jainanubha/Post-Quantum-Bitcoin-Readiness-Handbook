# P2TR, P2WSH and P2MR comparison

Specification basis: [BIP360 v0.12.1, pinned revision](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki), reviewed 13-11-2026.

## Output and validation properties

| Dimension | Native P2WSH | P2TR | P2MR (proposed) |
|---|---|---|---|
| Witness version | 0 | 1 | 2 |
| Program | SHA-256 of witness script | x-only tweaked secp256k1 key | Script-tree Merkle root |
| Program length | 32 B | 32 B | 32 B |
| scriptPubKey length | 34 B | 34 B | 34 B |
| Serialized output (amount + length + script) | 43 B | 43 B | 43 B |
| Address encoding | Bech32, bc1q | Bech32m, bc1p | Bech32m, bc1z |
| Key-path spend | None | Yes | None |
| Script path | Full witness script | Selected leaf and proof | Selected leaf and proof |
| Control-block length | Not applicable | 33 + 32d | 1 + 32d |
| Depth zero | Not applicable | Evaluated according to leaf version | Matching proof succeeds without executing script |
| Ordinary signature verification | ECDSA | Schnorr | Schnorr in enforced current-version leaves |
| Native Tapscript upgrade mechanisms | No | Yes | Proposed reuse |
| Consensus status | Deployed | Deployed | Draft |

## Security and operational comparison

| Dimension | P2WSH | P2TR | P2MR (proposed) |
|---|---|---|---|
| Persistent output-level ECC key | No | Yes | No |
| Long-exposure mitigation | Conditional on no independent disclosure of keys and scripts | Direct output key remains exposed | Conditional on no independent disclosure and safe leaves |
| Short-exposure risk of classical signatures | Remains when keys revealed | Remains; output key already exposed | Remains when leaf keys revealed |
| Native PQ signatures | No | No | Not introduced by BIP360 |
| Policy disclosure on ordinary script spend | Entire witness script | Selected script and path; internal key | Selected script and path |
| Cooperative signature-only spend | No native key path | Yes | No |
| Wallet backup | Script and keys | Internal key, keys and tree if used | Exact scripts, tree and keys |
| Upgrade safety | Existing deployed v0 rules | Avoid unrestricted future leaves/opcodes | Same cautions plus depth-zero success and activation status |

A hash commitment does not guarantee confidentiality of a low-entropy policy. Long exposure may occur through reused keys, descriptors or other disclosures. A stronger branch does not protect an independently usable weaker branch.

## Size examples

Matched current-version 34-byte Schnorr leaf, 64-byte signature, no annex. P2WSH is not included in this numerical comparison because its ECDSA script and signatures are not identical to the Tapscript leaf; compare a separately specified P2WSH template as in Chapter 11. Depth-zero P2MR is excluded from enforced-spend comparisons.

| Depth | P2TR raw control | P2MR raw control | P2TR witness | P2MR witness | Saving (bytes) |
|---:|---:|---:|---:|---:|---:|
| 1 | 65 | 33 | 167 | 135 | 32 |
| 2 | 97 | 65 | 199 | 167 | 32 |
| 4 | 161 | 129 | 263 | 231 | 32 |
| 7 | 257 | 225 | 361 | 327 | 34 |
| 8 | 289 | 257 | 393 | 361 | 32 |
| 128 | 4,129 | 4,097 | 4,233 | 4,201 | 32 |

For one input and two 34-byte output scripts, stripped size is 137 bytes and the marker/flag add two witness bytes. Weight = 550 + per-input witness size. Fees are illustrative, not current fee estimates.

| Spend format | Per-input witness (B) | Weight (WU) | vsize (vB) | Fee at 10 sat/vB |
|---|---:|---:|---:|---:|
| P2TR key path | 66 | 616 | 154 | 1,540 sat |
| P2TR script path, depth 1 | 167 | 717 | 180 | 1,800 sat |
| P2MR, depth 1 | 135 | 685 | 172 | 1,720 sat |
| P2TR script path, depth 7 | 361 | 911 | 228 | 2,280 sat |
| P2MR, depth 7 | 327 | 877 | 220 | 2,200 sat |
| P2TR script path, depth 8 | 393 | 943 | 236 | 2,360 sat |
| P2MR, depth 8 | 361 | 911 | 228 | 2,280 sat |

At depth 7 the CompactSize length prefix changes for P2TR but not P2MR, producing a 34-byte witness saving; otherwise the positive-depth saving is 32 bytes for matched scripts. The comparison does not include computational benchmarking or speculative future PQ signature encodings.

## References

- [BIP141](https://github.com/bitcoin/bips/blob/master/bip-0141.mediawiki)
- [BIP341](https://github.com/bitcoin/bips/blob/master/bip-0341.mediawiki)
- [BIP342](https://github.com/bitcoin/bips/blob/master/bip-0342.mediawiki)
- [BIP350](https://github.com/bitcoin/bips/blob/master/bip-0350.mediawiki)
- [BIP360](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki)
