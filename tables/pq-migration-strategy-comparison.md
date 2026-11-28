# Post-quantum migration strategy comparison

## Evaluation assumptions

Assume a future suitable PQ destination exists and distinguish long-exposure key recovery from key recovery fast enough to threaten a transaction in flight. A mechanism may be effective for one exposure class but not another. All entries below are conceptual or draft approaches, not currently enforced migration rules. No ranking or numeric security score is implied.

| Strategy | Mechanism | Potential benefit | Important limitation | Prerequisites / evidence |
| --- | --- | --- | --- | --- |
| Voluntary migration | Holders choose to spend into protected outputs | Preserves discretion and existing access | Residual exposed coins and late migration risk | Suitable activated PQ outputs; adoption and capacity model |
| Scheduled destination restrictions | Phase A of BIP361 | Discourages creation of further vulnerable outputs | Unready receivers, change and classification issues | Precisely specified output predicate and deployment |
| Scheduled legacy sunset with rescue | Phase B of pinned BIP361 | Could prevent authorization by recovered scalar alone | Honest holders without evidence may lose access | Secure rescue design and coverage estimates |
| Prepared commit/reveal | Earlier authorized commitment constrains later migration | Can reduce some reveal-time races | Requires preparation, binding and reliable inclusion; already-exposed keys need separate analysis | Consensus specification and adversarial analysis |
| Unprepared independent-secret rescue | Prove a secret not available from the compromised scalar | May preserve access for compatible wallets | No universal secret; proof cost and metadata exposure | Soundness, derivation linkage and wallet inventory |
| Hourglass-style rate limits | Limit movement of designated legacy outputs | May slow rapid redistribution | Not owner authentication; competition and delays | Separate activated rules; incentive analysis |
| Hybrid new-output authorization | Combine classical and PQ mechanisms under a defined policy | May diversify cryptographic assumptions | Does not retrofit legacy outputs; weak alternative branch may suffice | Secure composition and consensus integration |

## Compare by scenario

- **Before a CRQC:** voluntary migration and prepared commitments can exploit a longer planning window, provided new destinations are safe. Scheduled restrictions may create incentives but also coordination burdens.
- **Long exposure capability:** an exposed scalar may already be compromised. A proof based solely on that scalar cannot authenticate original ownership. Rate limiting changes capacity, not this authentication property.
- **Short exposure capability:** even a previously hidden key can become a target when a legacy spend is revealed. A binding prior commitment or independent recovery evidence may help under its stated assumptions.
- **Lost or unsupported recovery material:** no proposed mechanism should be assumed to recover every output. Evaluate legitimate access and theft prevention separately.

## References

- [BIP361](https://github.com/bitcoin/bips/blob/da7cc678d522c7ff09d3d4e3b5de1a1ccc1e90c9/bip-0361.mediawiki)
- [Post-Quantum Migrations via Commit-then-Reveal — Delving Bitcoin](https://delvingbitcoin.org/t/post-quantum-migrations-via-commit-then-reveal-ala-bip16-an-alternative-to-sunsetting/2467)
- [Commit-Reveal for PQ Migration — Delving Bitcoin](https://delvingbitcoin.org/t/commit-reveal-for-pq-migration/2419/1)
- [Hourglass spending rules — draft in author repository](https://github.com/cryptoquick/bips/blob/hourglass/bip-hourglass.mediawiki)
- [Post Quantum Recovery Framework — Delving Bitcoin, November 20, 2026](https://delvingbitcoin.org/t/post-quantum-recovery-framework/3099/1)
- [BIP360](https://github.com/bitcoin/bips/blob/e7263a4cfe500c89e4269889244606953691ca33/bip-0360.mediawiki)
