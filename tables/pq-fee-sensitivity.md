# Fee sensitivity by authorization template

Classical rows use current input authorization; PQ rows use M1 (raw signature and public key in two witness items). All transactions have native empty scriptSig, two 34-byte output scripts, no annex or extra metadata; classical P2WPKH assumes 72-byte transaction signature and 33-byte key. Raw PQ signatures exclude any future sighash or mode byte. Falcon padded candidate encodings are not final FN-DSA parameters. All fee columns are satoshis at illustrative rates, not network quotes.

## 1 input(s), two outputs

| Input authorization | Weight (WU) | Virtual size (vB) | 1 sat/vB | 5 sat/vB | 10 sat/vB | 50 sat/vB |
| --- | --- | --- | --- | --- | --- | --- |
| P2TR key path | 616 | 154 | 154 | 770 | 1,540 | 7,700 |
| ML-DSA-44 | 4,289 | 1,073 | 1,073 | 5,365 | 10,730 | 53,650 |
| ML-DSA-65 | 5,818 | 1,455 | 1,455 | 7,275 | 14,550 | 72,750 |
| ML-DSA-87 | 7,776 | 1,944 | 1,944 | 9,720 | 19,440 | 97,200 |
| SLH-DSA-128s | 8,443 | 2,111 | 2,111 | 10,555 | 21,110 | 105,550 |
| SLH-DSA-128f | 17,675 | 4,419 | 4,419 | 22,095 | 44,190 | 220,950 |
| Falcon-512 padded candidate | 2,120 | 530 | 530 | 2,650 | 5,300 | 26,500 |

## 5 input(s), two outputs

| Input authorization | Weight (WU) | Virtual size (vB) | 1 sat/vB | 5 sat/vB | 10 sat/vB | 50 sat/vB |
| --- | --- | --- | --- | --- | --- | --- |
| P2TR key path | 1,536 | 384 | 384 | 1,920 | 3,840 | 19,200 |
| ML-DSA-44 | 19,901 | 4,976 | 4,976 | 24,880 | 49,760 | 248,800 |
| ML-DSA-65 | 27,546 | 6,887 | 6,887 | 34,435 | 68,870 | 344,350 |
| ML-DSA-87 | 37,336 | 9,334 | 9,334 | 46,670 | 93,340 | 466,700 |
| SLH-DSA-128s | 40,671 | 10,168 | 10,168 | 50,840 | 101,680 | 508,400 |
| SLH-DSA-128f | 86,831 | 21,708 | 21,708 | 108,540 | 217,080 | 1,085,400 |
| Falcon-512 padded candidate | 9,056 | 2,264 | 2,264 | 11,320 | 22,640 | 113,200 |

## 10 input(s), two outputs

| Input authorization | Weight (WU) | Virtual size (vB) | 1 sat/vB | 5 sat/vB | 10 sat/vB | 50 sat/vB |
| --- | --- | --- | --- | --- | --- | --- |
| P2TR key path | 2,686 | 672 | 672 | 3,360 | 6,720 | 33,600 |
| ML-DSA-44 | 39,416 | 9,854 | 9,854 | 49,270 | 98,540 | 492,700 |
| ML-DSA-65 | 54,706 | 13,677 | 13,677 | 68,385 | 136,770 | 683,850 |
| ML-DSA-87 | 74,286 | 18,572 | 18,572 | 92,860 | 185,720 | 928,600 |
| SLH-DSA-128s | 80,956 | 20,239 | 20,239 | 101,195 | 202,390 | 1,011,950 |
| SLH-DSA-128f | 173,276 | 43,319 | 43,319 | 216,595 | 433,190 | 2,165,950 |
| Falcon-512 padded candidate | 17,726 | 4,432 | 4,432 | 22,160 | 44,320 | 221,600 |

## Sources and calculation

See [transaction-weight comparison](pq-transaction-weight-comparison.md) and [BIP141](https://bips.dev/141/). Fee = ceiling(rate × rounded transaction vsize).
