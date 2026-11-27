# Post-quantum signature evaluation matrix

| Dimension | ML-DSA | SLH-DSA | Falcon | SHRINCS |
| --- | --- | --- | --- | --- |
| Specification | FIPS 204 | FIPS 205 | Candidate; FIPS 206 in development | Research proposal |
| Assumption family | Module lattices | Hash functions | NTRU lattices | Hash functions |
| Persistent signing counter | No | No | No | Yes, for compact stateful path |
| Raw data sizes | [Size table](signature-size-comparison.md) | [Size table](signature-size-comparison.md) | [Candidate size table](signature-size-comparison.md) | Revision and path dependent |
| Valid/invalid verification latency | Not measured here | Not measured here | Not measured here | Not measured here |
| Peak verifier memory | Not measured here | Not measured here | Not measured here | Not measured here |
| Multiparty protocol security | Research required | Research required | Research required | Research required |
| Consensus integration | New rules needed | New rules needed | New rules needed | New rules needed |
| Wallet recovery model | Define seed and metadata | Define seed and metadata | Define seed and metadata | Define state loss and fallback |
| Overall suitability | Insufficient evidence for a universal rating | Insufficient evidence | Insufficient evidence | Insufficient evidence |

## References

- [FIPS 204](https://csrc.nist.gov/pubs/fips/204/final)
- [FIPS 205](https://csrc.nist.gov/pubs/fips/205/final)
- [Falcon](https://falcon-sign.info/)
- [NIST PQC standardization status](https://csrc.nist.gov/Projects/post-quantum-cryptography/post-quantum-cryptography-standardization)
- [SHRINCS research discussion](https://delvingbitcoin.org/t/shrincs-324-byte-stateful-post-quantum-signatures-with-static-backups/2158)
