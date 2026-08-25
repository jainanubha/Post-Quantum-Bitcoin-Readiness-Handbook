# Why Post-Quantum Readiness Matters for Bitcoin

As a decentralized system, Bitcoin is designed to survive without trusted intermediaries. Users do not rely on a central bank, payment processor, custodian or administrator to approve valid transactions. Instead, ownership is enforced by cryptography, consensus rules, economic incentives and independently validating nodes.

This makes Bitcoin powerful, but it also creates a long-term responsibility: the cryptographic assumptions that protect Bitcoin must remain credible over time.

Post-quantum readiness matters because Bitcoin uses elliptic-curve public-private keys and digital signatures to authorize spending. Today, those signatures are secure against known classical attacks when used correctly. However, a sufficiently capable quantum computer could threaten the hard mathematical problems on which ECDSA and Schnorr signatures rely. That possibility does not mean Bitcoin is immediately broken. It does mean that Bitcoin developers, educators, wallet builders, exchanges, custodians and users need to understand the risk early enough to prepare responsibly.

The goal of post-quantum readiness now is to avoid being forced into rushed decisions later.

A common mistake is to treat quantum risk as a binary question:

```text
Is Bitcoin broken today?
```

A better question is:

```text
What would Bitcoin need to understand, build, review, test and coordinate before a cryptographically relevant quantum computer exists?
```

Bitcoin does not need alarmist claims to justify post-quantum study. The reason readiness matters is that Bitcoin changes slowly, deliberately and through broad review. That is a feature, not a bug. But it also means that migration cannot begin only after a practical attack already exists.

A responsible readiness effort should:

* explain the risk model clearly
* distinguish realistic concerns from hype
* identify which parts of Bitcoin are affected
* track current standards and proposals
* evaluate tradeoffs before they become urgent
* help wallets and infrastructure prepare
* create educational material that improves public understanding

This handbook is part of that readiness work.

## Bitcoin has a long security horizon

Bitcoin is not a short-lived application. Many users expect Bitcoin to preserve value and transaction validity across decades. Coins can remain unspent for long periods. Some UTXOs may not move for years. Some keys may be stored in cold wallets. Some coins may belong to individuals, companies, exchanges, custodians, estates, treasuries or long-term generational savings plans.

This long time horizon changes how security should be considered.

A cryptographic system used only for a short session can often be replaced quickly. Bitcoin cannot be replaced so easily. Its ledger is public, permanent and globally replicated. Its old outputs continue to exist until they are spent. Its consensus rules protect all users at once. A weakness affecting old coins, exposed public keys or transaction authorization cannot be handled like a normal software patch.

Post-quantum readiness matters because Bitcoin’s history cannot be rewritten and its UTXO set cannot be migrated centrally.

## Bitcoin’s public ledger makes exposure visible

Bitcoin’s transparency is essential for verification. Anyone can verify the supply, inspect transactions, validate signatures and check that coins are not spent twice. But transparency also means that some information is permanently visible.

This is especially important for public keys.

In Bitcoin, a private key authorizes spending by producing a valid digital signature. A public key allows the network to verify that signature. If a public key is visible and if a future quantum computer can derive the corresponding private key from that public key, then coins controlled by that key may be at risk.

Not all Bitcoin outputs expose public keys in the same way.

Some outputs reveal a public key directly. Some hide the public key behind a hash until the coin is spent. Taproot outputs commit to an x-only public key in the output itself. Script-path constructions may expose different data depending on how they are spent. Reused addresses can create additional exposure because a public key revealed once may remain relevant to other coins controlled by the same key.

This is why post-quantum Bitcoin analysis must study output types and public-key exposure together.

## The main threat is to signatures, not immediately to mining

Quantum computing does not threaten every part of Bitcoin in the same way.

Bitcoin uses several cryptographic tools, including:

* digital signatures
* hash functions
* Merkle trees
* proof-of-work
* commitments
* scripts
* address encodings

Quantum algorithms affect these tools differently.

The most direct post-quantum concern for Bitcoin is the security of elliptic-curve signatures. Bitcoin’s older signatures use ECDSA over elliptic curve secp256k1. Taproot uses Schnorr signatures over the same elliptic curve. Both rely on the hardness of the elliptic-curve discrete logarithm problem (ECDLP).

A sufficiently capable quantum computer running Shor’s algorithm could, in principle, derive a private key from a corresponding public key. That is the core risk for Bitcoin ownership.

By contrast, Bitcoin mining is based on hash-based proof-of-work. Quantum search algorithms may offer speedups against brute-force search, but this is a different and less direct risk than recovering private keys from public keys.

## Public-key exposure creates different attack windows

Post-quantum risk in Bitcoin depends heavily on when a public key becomes visible and how long it remains useful to an attacker.

A useful distinction is:

```text
Long-exposure attack:
The attacker has access to a public key for a long period before the coin is moved.

Short-exposure attack:
The attacker sees the public key only when a transaction is broadcast and must act before that transaction confirms.
```

This distinction matters because different defenses may address different attack windows.

A long-exposure attack may target coins whose public keys are already visible on-chain. Examples include old pay-to-public-key outputs, reused keys or outputs whose public key is visible before spending. If a future attacker can derive private keys from those visible public keys, they may not need to race the mempool. They can compute privately and spend later.

A short-exposure attack is different. For many outputs, the public key may be hidden until the owner broadcasts a spending transaction. Once the transaction enters the mempool, the public key becomes visible. A very fast quantum attacker could attempt to derive the private key and broadcast a conflicting transaction before confirmation of the original spend.

These two models lead to different design questions:

* Can Bitcoin reduce long-term public-key exposure?
* Can wallets avoid address reuse and unnecessary key exposure?
* Can future output types avoid exposing elliptic-curve public keys?
* Would post-quantum signatures be required to protect spend-time authorization?
* How should migration handle coins whose public keys are already exposed?
* What happens to dormant or lost coins?

Post-quantum readiness matters because these questions cannot be answered only by choosing a new signature algorithm.

## Bitcoin migration is a coordination problem

Even if the Bitcoin community identified a good post-quantum signature scheme, migration would still be difficult.

A practical migration would need coordination across:

* consensus rules
* full nodes
* miners
* wallets
* hardware wallets
* exchanges
* custodians
* Bitcoin libraries
* block explorers
* educational materials
* user behavior

Every layer has different constraints.

A full node must be able to validate new rules safely. A wallet must generate and back up new keys correctly. A hardware wallet must sign securely and display meaningful information to users. Exchanges and custodians must migrate many UTXOs without creating operational risk. Users must understand what to do without being misled by scams.

This is why readiness must begin with education and tooling. If the ecosystem waits until migration is urgent, users may face confusion, high fees, phishing campaigns and unsafe shortcuts.

Bitcoin’s decentralized nature makes preparation especially important.

## Post-quantum signatures have Bitcoin-specific costs

Post-quantum signature schemes are not drop-in replacements for Bitcoin signatures.

A signature scheme that is acceptable for a web protocol or enterprise application may still be difficult for Bitcoin. Bitcoin has scarce block space. Every byte added to a transaction affects fees, mempool behavior and block capacity. Verification cost also matters because every fully validating node must check the rules.

Important Bitcoin-specific questions include:

* How large are the public keys?
* How large are the signatures?
* How expensive is verification?
* How much witness data would be added?
* How would fees change for users?
* Can the scheme support multisig or threshold use cases?
* Can hardware wallets implement it safely?
* Does signing require dangerous state management?
* Can wallets back up keys reliably?
* How much review has the new implementation received?

This is why the handbook studies signature-size comparisons, transaction-weight estimates, UTXO exposure and developer labs. These tools help readers understand that post-quantum Bitcoin migration is a systems problem, not only a cryptography problem.

## Standards are necessary, but not sufficient

The finalization of post-quantum cryptography standards is a major milestone for the broader security ecosystem. Standards such as ML-DSA and SLH-DSA are important because they give developers and researchers concrete schemes to study, benchmark and compare.

However, standards alone do not answer the Bitcoin question.

Bitcoin must ask:

* Which standardized schemes are suitable for consensus validation?
* Which schemes have acceptable signature and public-key sizes?
* Which schemes are easiest to implement safely?
* Which schemes are conservative enough for long-term use?
* Which schemes can be used by wallets without complicating UX?
* Which schemes allow practical migration from existing UTXOs?
* How should Bitcoin handle old ECDSA and Schnorr outputs?

A NIST-standardized signature scheme may be a serious candidate for study, but Bitcoin adoption would require separate analysis, specification, implementation, review, activation discussion and ecosystem migration.

## Current proposals show that the discussion has already started

The Bitcoin community is already discussing output types, migration policies, post-quantum signatures, hash-based signatures, Taproot fallback paths, recovery mechanisms and wallet-readiness approaches.

Two draft BIPs are especially useful study points.

### BIP360: Pay-to-Merkle-Root

BIP360 proposes Pay-to-Merkle-Root (P2MR). It can be understood as a Taproot-like script-tree output without the Taproot key-path spend.

The purpose is to reduce long-term exposure to elliptic-curve public keys. P2MR does not by itself introduce a post-quantum signature scheme. Instead, it gives readers a concrete example of how Bitcoin output design might reduce one part of the quantum-risk surface.

For this handbook, BIP360 is important because it teaches the difference between:

* output design
* public-key exposure
* script paths
* key paths
* long-exposure risk
* post-quantum signatures

### BIP361: Post Quantum Migration and Legacy Signature Sunset

BIP361 discusses migration and the possible sunset of legacy ECDSA and Schnorr signatures after a post-quantum output type exists.

It is important because it shows that post-quantum readiness is not only about technical primitives. It also raises questions about incentives, timelines, old coins and rescue mechanisms.

For this handbook, BIP361 is useful because it forces readers to ask:

* Should migration be voluntary forever?
* Should sending to vulnerable address types eventually be restricted?
* Should old exposed-key coins remain spendable indefinitely?
* How can Bitcoin prevent quantum theft without violating user expectations?
* What kind of rescue mechanisms are technically and socially acceptable?

These questions are difficult, and the handbook should not pretend they are settled.

## Wallet readiness matters before consensus change

Even before Bitcoin adopts any post-quantum change, wallets can begin improving readiness.

Wallets can help users by:

* avoiding address reuse
* explaining public-key exposure
* supporting modern address types
* reducing unnecessary key reuse
* tracking which outputs have exposed public keys
* improving backup and recovery practices
* preparing for future output types
* educating their users

Wallets will be central to any future migration. If users eventually need to move coins into new output types, wallets must make that process safe, understandable and hard to misuse.

A future post-quantum migration could fail socially even if it succeeds technically, if users do not understand what they need to do or if wallet support is fragmented.

Readiness therefore includes wallet UX and education.

## Exchanges and custodians

Large exchanges and custodians may control many UTXOs. They may also have operational constraints that individual users do not face.

They must consider:

* how many UTXOs they control
* which keys or public keys are already exposed
* how quickly they can migrate
* how fees affect large migrations
* how withdrawal addresses are supported
* how internal signing systems would change
* how hardware security modules or signing infrastructure would adapt
* how to communicate these changes to their users

A post-quantum migration that ignores exchanges and custodians could create congestion, inconsistent support and user confusion.

## Responsibility of educators

Quantum risk is easy to misunderstand.

Some people may overstate the threat and claim that Bitcoin is already doomed. Others may dismiss the topic entirely because practical quantum attacks do not exist today. Both approaches are unhelpful.

Good education should explain:

* what is known
* what is uncertain
* what is standardized
* what is only proposed
* what is deployed
* what remains experimental
* what Bitcoin-specific tradeoffs matter

Educators should be especially careful with timelines. A research estimate or corporate migration target should not be presented as a guaranteed date for a Bitcoin-breaking quantum computer. At the same time, uncertainty should not be used as an excuse to avoid preparation.

The balanced position is:

```text
Bitcoin is not currently broken by quantum computers, but the ecosystem should begin serious readiness work because migration will take time.
```

## Readiness protects confidence

Bitcoin’s value depends partly on confidence in its rules and security model. Even before a practical quantum attack, credible fear of future key recovery could influence user behavior, institutional decisions, custody practices and long-term adoption and retention.

If Bitcoin has no clear educational resources, no proposal map, no wallet-readiness discussion, and no shared vocabulary, the community may be more vulnerable to panic when new quantum milestones are announced.

## Summary

Post-quantum readiness matters for Bitcoin because:

1. Bitcoin relies on elliptic-curve signatures for ownership.
2. A future cryptographically relevant quantum computer could threaten exposed public keys.
3. Bitcoin’s public ledger makes some exposure permanent.
4. Different output types have different risk profiles.
5. Long-exposure and short-exposure attacks require different analysis.
6. Post-quantum signatures introduce size, fee, validation and UX tradeoffs.
7. Bitcoin migration requires broad ecosystem coordination.
8. Draft proposals such as BIP360 and BIP361 show that the discussion is already active.
9. Wallets, exchanges, custodians and educators all have readiness roles.
10. Good preparation can reduce panic and improve future review.

The right approach is careful, public, technically grounded readiness.
