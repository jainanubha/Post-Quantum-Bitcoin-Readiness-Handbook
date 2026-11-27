# Signature-size comparison

Expanded private-key sizes do not necessarily equal seed storage. SHA2 and SHAKE SLH-DSA profiles share encoded sizes but are distinct variants. Transaction-level overhead, signature hash modes, script proofs and serialization are excluded.

| Scheme / parameter | NIST category | Public key (B) | Encoded private key (B) | Signature (B) | Signing restriction / status |
| --- | --- | ---: | ---: | ---: | --- |
| ECDSA secp256k1 (compressed key) | — | 33 | 32 scalar | DER ≤72 | Classical Bitcoin baseline |
| BIP340 Schnorr | — | 32 | 32 scalar | 64 | Classical Bitcoin baseline |
| ML-DSA-44 | 2 | 1,312 | 2,560 | 2,420 | Stateless; FIPS 204 |
| ML-DSA-65 | 3 | 1,952 | 4,032 | 3,309 | Stateless; FIPS 204 |
| ML-DSA-87 | 5 | 2,592 | 4,896 | 4,627 | Stateless; FIPS 204 |
| SLH-DSA-SHA2-128s / SHAKE-128s | 1 | 32 | 64 | 7,856 | Stateless; FIPS 205 |
| SLH-DSA-SHA2-128f / SHAKE-128f | 1 | 32 | 64 | 17,088 | Stateless; FIPS 205 |
| SLH-DSA-SHA2-192s / SHAKE-192s | 3 | 48 | 96 | 16,224 | Stateless; FIPS 205 |
| SLH-DSA-SHA2-192f / SHAKE-192f | 3 | 48 | 96 | 35,664 | Stateless; FIPS 205 |
| SLH-DSA-SHA2-256s / SHAKE-256s | 5 | 64 | 128 | 29,792 | Stateless; FIPS 205 |
| SLH-DSA-SHA2-256f / SHAKE-256f | 5 | 64 | 128 | 49,856 | Stateless; FIPS 205 |
| Falcon-512 padded candidate | 1 (candidate) | 897 | 1,281 | 666 | Stateless; candidate format |
| Falcon-1024 padded candidate | 5 (candidate) | 1,793 | 2,305 | 1,280 | Stateless; candidate format |

## Additional hash-based examples

| Scheme | Size considerations | Status / restrictions |
| --- | --- | --- |
| Lamport OTS | For a simple 256-bit digest and 32-byte secrets, raw revealed secrets occupy 8,192 bytes, and 512 raw public hash values occupy 16,384 bytes before commitments or compression | One-time key; educational example, no NIST category asserted |
| WOTS+ | Depends on hash length and Winternitz parameter | One-time key; component construction |
| XMSS / LMS | Depends on tree height and one-time signature parameters | Stateful; specified variants in SP 800-208 |
| SHRINCS | Original research discussion reports 324-byte compact stateful signatures under particular assumptions; fallback differs | Research proposal; stateful path with stateless recovery |
| SHRIMPS | Original research discussion reports roughly 2.5 KB under limited-use assumptions | Research proposal; device and signing-volume assumptions |

Falcon values refer to the candidate padded format rather than a finalized FIPS 206 encoding. ECDSA DER and BIP340 raw signatures exclude Bitcoin sighash bytes. Private-key scalar lengths are raw representations, not wallet file sizes. SLH-DSA has specified signing-volume security bounds despite stateless operation.

## References

- [BIP 66](https://bips.dev/66/) and [BIP 340](https://bips.dev/340/)
- [FIPS 204, Section 4, Tables 1–2](https://csrc.nist.gov/pubs/fips/204/final)
- [FIPS 205, Section 11, Table 2](https://csrc.nist.gov/pubs/fips/205/final)
- [Falcon candidate implementation encoding documentation](https://falcon-sign.info/impl/falcon.h.html)
- [SP 800-208](https://csrc.nist.gov/pubs/sp/800/208/final)
- [SHRINCS](https://delvingbitcoin.org/t/shrincs-324-byte-stateful-post-quantum-signatures-with-static-backups/2158) and [SHRIMPS](https://delvingbitcoin.org/t/shrimps-2-5-kb-post-quantum-signatures-across-multiple-stateful-devices/2355)
