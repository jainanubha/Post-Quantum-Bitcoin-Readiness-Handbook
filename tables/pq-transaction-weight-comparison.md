# Post-quantum transaction-weight comparison

Classical rows use current input authorization; PQ rows use M1 (raw signature and public key in two witness items). All transactions have native empty scriptSig, two 34-byte output scripts, no annex or extra metadata; classical P2WPKH assumes 72-byte transaction signature and 33-byte key. Raw PQ signatures exclude any future sighash or mode byte. Falcon padded candidate encodings are not final FN-DSA parameters.

| Inputs | Authorization | Status | Sig (B) | Revealed key (B) | Base (B) | Witness delta (B) | Total (B) | Weight (WU) | vsize (vB) | Fee at 10 sat/vB (sat) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | P2TR key path | Current | 64 | — | 137 | 68 | 205 | 616 | 154 | 1,540 |
| 1 | P2WPKH | Current | 72 | 33 | 137 | 110 | 247 | 658 | 165 | 1,650 |
| 1 | ML-DSA-44 | Hypothetical | 2420 | 1312 | 137 | 3,741 | 3,878 | 4,289 | 1,073 | 10,730 |
| 1 | ML-DSA-65 | Hypothetical | 3309 | 1952 | 137 | 5,270 | 5,407 | 5,818 | 1,455 | 14,550 |
| 1 | ML-DSA-87 | Hypothetical | 4627 | 2592 | 137 | 7,228 | 7,365 | 7,776 | 1,944 | 19,440 |
| 1 | SLH-DSA-128s | Hypothetical | 7856 | 32 | 137 | 7,895 | 8,032 | 8,443 | 2,111 | 21,110 |
| 1 | SLH-DSA-128f | Hypothetical | 17088 | 32 | 137 | 17,127 | 17,264 | 17,675 | 4,419 | 44,190 |
| 1 | SLH-DSA-192s | Hypothetical | 16224 | 48 | 137 | 16,279 | 16,416 | 16,827 | 4,207 | 42,070 |
| 1 | SLH-DSA-192f | Hypothetical | 35664 | 48 | 137 | 35,719 | 35,856 | 36,267 | 9,067 | 90,670 |
| 1 | SLH-DSA-256s | Hypothetical | 29792 | 64 | 137 | 29,863 | 30,000 | 30,411 | 7,603 | 76,030 |
| 1 | SLH-DSA-256f | Hypothetical | 49856 | 64 | 137 | 49,927 | 50,064 | 50,475 | 12,619 | 126,190 |
| 1 | Falcon-512 padded candidate | Hypothetical candidate | 666 | 897 | 137 | 1,572 | 1,709 | 2,120 | 530 | 5,300 |
| 1 | Falcon-1024 padded candidate | Hypothetical candidate | 1280 | 1793 | 137 | 3,082 | 3,219 | 3,630 | 908 | 9,080 |
| 5 | P2TR key path | Current | 64 | — | 301 | 332 | 633 | 1,536 | 384 | 3,840 |
| 5 | P2WPKH | Current | 72 | 33 | 301 | 542 | 843 | 1,746 | 437 | 4,370 |
| 5 | ML-DSA-44 | Hypothetical | 2420 | 1312 | 301 | 18,697 | 18,998 | 19,901 | 4,976 | 49,760 |
| 5 | ML-DSA-65 | Hypothetical | 3309 | 1952 | 301 | 26,342 | 26,643 | 27,546 | 6,887 | 68,870 |
| 5 | ML-DSA-87 | Hypothetical | 4627 | 2592 | 301 | 36,132 | 36,433 | 37,336 | 9,334 | 93,340 |
| 5 | SLH-DSA-128s | Hypothetical | 7856 | 32 | 301 | 39,467 | 39,768 | 40,671 | 10,168 | 101,680 |
| 5 | SLH-DSA-128f | Hypothetical | 17088 | 32 | 301 | 85,627 | 85,928 | 86,831 | 21,708 | 217,080 |
| 5 | SLH-DSA-192s | Hypothetical | 16224 | 48 | 301 | 81,387 | 81,688 | 82,591 | 20,648 | 206,480 |
| 5 | SLH-DSA-192f | Hypothetical | 35664 | 48 | 301 | 178,587 | 178,888 | 179,791 | 44,948 | 449,480 |
| 5 | SLH-DSA-256s | Hypothetical | 29792 | 64 | 301 | 149,307 | 149,608 | 150,511 | 37,628 | 376,280 |
| 5 | SLH-DSA-256f | Hypothetical | 49856 | 64 | 301 | 249,627 | 249,928 | 250,831 | 62,708 | 627,080 |
| 5 | Falcon-512 padded candidate | Hypothetical candidate | 666 | 897 | 301 | 7,852 | 8,153 | 9,056 | 2,264 | 22,640 |
| 5 | Falcon-1024 padded candidate | Hypothetical candidate | 1280 | 1793 | 301 | 15,402 | 15,703 | 16,606 | 4,152 | 41,520 |
| 10 | P2TR key path | Current | 64 | — | 506 | 662 | 1,168 | 2,686 | 672 | 6,720 |
| 10 | P2WPKH | Current | 72 | 33 | 506 | 1,082 | 1,588 | 3,106 | 777 | 7,770 |
| 10 | ML-DSA-44 | Hypothetical | 2420 | 1312 | 506 | 37,392 | 37,898 | 39,416 | 9,854 | 98,540 |
| 10 | ML-DSA-65 | Hypothetical | 3309 | 1952 | 506 | 52,682 | 53,188 | 54,706 | 13,677 | 136,770 |
| 10 | ML-DSA-87 | Hypothetical | 4627 | 2592 | 506 | 72,262 | 72,768 | 74,286 | 18,572 | 185,720 |
| 10 | SLH-DSA-128s | Hypothetical | 7856 | 32 | 506 | 78,932 | 79,438 | 80,956 | 20,239 | 202,390 |
| 10 | SLH-DSA-128f | Hypothetical | 17088 | 32 | 506 | 171,252 | 171,758 | 173,276 | 43,319 | 433,190 |
| 10 | SLH-DSA-192s | Hypothetical | 16224 | 48 | 506 | 162,772 | 163,278 | 164,796 | 41,199 | 411,990 |
| 10 | SLH-DSA-192f | Hypothetical | 35664 | 48 | 506 | 357,172 | 357,678 | 359,196 | 89,799 | 897,990 |
| 10 | SLH-DSA-256s | Hypothetical | 29792 | 64 | 506 | 298,612 | 299,118 | 300,636 | 75,159 | 751,590 |
| 10 | SLH-DSA-256f | Hypothetical | 49856 | 64 | 506 | 499,252 | 499,758 | 501,276 | 125,319 | 1,253,190 |
| 10 | Falcon-512 padded candidate | Hypothetical candidate | 666 | 897 | 506 | 15,702 | 16,208 | 17,726 | 4,432 | 44,320 |
| 10 | Falcon-1024 padded candidate | Hypothetical candidate | 1280 | 1793 | 506 | 30,802 | 31,308 | 32,826 | 8,207 | 82,070 |

## Method

See Chapter 11, Sections 11.2, 11.6 and 11.7. Each input contributes its own witness vector; marker and flag add two shared bytes. BIP141 vsize is rounded once for each transaction. SLH-DSA SHA2 and SHAKE variants have equal sizes for matching profiles.

## Sources

- [BIP 141 — Segregated Witness](https://bips.dev/141/)
- [BIP 144 — SegWit peer services and serialization](https://bips.dev/144/)
- [BIP 341 — Taproot spending rules](https://bips.dev/341/)
- [FIPS 204 — ML-DSA, section 4](https://csrc.nist.gov/pubs/fips/204/final)
- [FIPS 205 — SLH-DSA, section 11](https://csrc.nist.gov/pubs/fips/205/final)
- [Falcon candidate encoding documentation](https://falcon-sign.info/impl/falcon.h.html)
