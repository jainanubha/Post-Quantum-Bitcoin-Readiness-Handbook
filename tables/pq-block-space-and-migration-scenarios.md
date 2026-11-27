# Block-space and migration scenarios

Classical rows use current input authorization; PQ rows use M1 (raw signature and public key in two witness items). All transactions have native empty scriptSig, two 34-byte output scripts, no annex or extra metadata; classical P2WPKH assumes 72-byte transaction signature and 33-byte key. Raw PQ signatures exclude any future sighash or mode byte. Falcon padded candidate encodings are not final FN-DSA parameters.

## Weight-only homogeneous capacity

Assume 4,000,000 WU block limit, 40,000 WU illustrative reserve, 600-second mean block interval. These are packing scenarios, not expected throughput or policy-valid transactions.

| Authorization | WU/tx | Ordinary tx/block | Tx/s (model) |
| --- | --- | --- | --- |
| P2TR key path | 616 | 6,428 | 10.71 |
| P2WPKH | 658 | 6,018 | 10.03 |
| ML-DSA-44 | 4,289 | 923 | 1.54 |
| ML-DSA-65 | 5,818 | 680 | 1.13 |
| ML-DSA-87 | 7,776 | 509 | 0.85 |
| SLH-DSA-128s | 8,443 | 469 | 0.78 |
| SLH-DSA-128f | 17,675 | 224 | 0.37 |
| SLH-DSA-192s | 16,827 | 235 | 0.39 |
| SLH-DSA-192f | 36,267 | 109 | 0.18 |
| SLH-DSA-256s | 30,411 | 130 | 0.22 |
| SLH-DSA-256f | 50,475 | 78 | 0.13 |
| Falcon-512 padded candidate | 2,120 | 1,867 | 3.11 |
| Falcon-1024 padded candidate | 3,630 | 1,090 | 1.82 |

## One-to-one migration of existing P2TR inputs

This independent model spends one old P2TR key-path input into one new 34-byte output, 444 WU and 111 vB. No PQ signature is used at this stage. Allocation is a fraction of the assumed ordinary-transaction budget; no protocol reserves that space.

| UTXOs | Budget share | Tx/block | Blocks (minimum in model) | Days at 600 s/block | Total fees at 10 sat/vB (BTC) |
| --- | --- | --- | --- | --- | --- |
| 100,000 | 5% | 445 | 225 | 1.562 | 1.1100 |
| 100,000 | 10% | 891 | 113 | 0.785 | 1.1100 |
| 100,000 | 50% | 4,459 | 23 | 0.160 | 1.1100 |
| 100,000 | 100% | 8,918 | 12 | 0.083 | 1.1100 |
| 1,000,000 | 5% | 445 | 2,248 | 15.611 | 11.1000 |
| 1,000,000 | 10% | 891 | 1,123 | 7.799 | 11.1000 |
| 1,000,000 | 50% | 4,459 | 225 | 1.562 | 11.1000 |
| 1,000,000 | 100% | 8,918 | 113 | 0.785 | 11.1000 |
| 10,000,000 | 5% | 445 | 22,472 | 156.056 | 111.0000 |
| 10,000,000 | 10% | 891 | 11,224 | 77.944 | 111.0000 |
| 10,000,000 | 50% | 4,459 | 2,243 | 15.576 | 111.0000 |
| 10,000,000 | 100% | 8,918 | 1,122 | 7.792 | 111.0000 |

## Sources

- [BIP 141 — Segregated Witness](https://bips.dev/141/)
- [BIP 341 — Taproot spending rules](https://bips.dev/341/)
- [FIPS 204 — ML-DSA, section 4](https://csrc.nist.gov/pubs/fips/204/final)
- [FIPS 205 — SLH-DSA, section 11](https://csrc.nist.gov/pubs/fips/205/final)
- [Falcon candidate encoding documentation](https://falcon-sign.info/impl/falcon.h.html)
- [Bitcoin Optech — Quantum resistance](https://bitcoinops.org/en/topics/quantum-resistance/)
