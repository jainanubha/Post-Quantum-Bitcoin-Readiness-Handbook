# Bitcoin Cryptography Refresher

Bitcoin combines several cryptographic mechanisms such as digital signatures that authorize spending, hash functions which provide identifiers, commitments, integrity checks and proof-of-work. It also uses merkle trees that allow efficient commitments to large sets of data. Bitcoin Script defines the conditions under which an output may be spent. Understanding these distinctions is essential for studying post-quantum Bitcoin readiness. A future quantum computer would not affect every cryptographic primitive in the same way. Elliptic-curve based digital signatures are exposed to a different class of attack than the cryptographic hash functions. An output whose public key is visible has a different risk profile from one whose public key remains hidden until spending. 

This chapter provides the cryptographic background needed for the later quantum-risk chapters. It is not intended to be a complete introduction to elliptic-curve cryptography or Bitcoin consensus. Instead, it focuses on the concepts most relevant to:

- Bitcoin transaction authorization,
- public-key exposure,
- ECDSA and Schnorr signatures,
- hash functions,
- commitments,
- Bitcoin Script,
- Taproot,
- and post-quantum migration.

## 1. Learning objectives

After reading this chapter, a reader should be able to explain:

1. The difference between a Bitcoin private key and public key.
2. Why Bitcoin uses the secp256k1 elliptic curve.
3. What security assumption protects ECDSA and Schnorr signatures.
4. How ECDSA and Schnorr signatures authorize Bitcoin spending.
5. Why secure nonce generation is essential.
6. How Bitcoin uses SHA-256, HASH160, and tagged hashes.
7. The difference between a signature, a hash and a commitment.
8. How Bitcoin Script and witness data express spending conditions.
9. The difference between Taproot key-path and script-path spending.
10. Why public-key exposure matters for post-quantum readiness.

## 2. Cryptography in Bitcoin’s security model

Bitcoin's security model combines several cryptographic components:

| Mechanism | Role in Bitcoin |
|---|---|
| Digital signatures | Authorize spending of transaction outputs |
| Hash functions | Create identifiers, commitments and proof-of-work challenges |
| Merkle trees | Commit efficiently to collections of transactions or scripts |
| Bitcoin Script | Define conditions that must be satisfied to spend an output |
| Proof of work | Order blocks and make chain history costly to rewrite |
| Consensus validation | Ensure all valid nodes apply the same rules |

This separation becomes important in post-quantum analysis. Shor’s algorithm primarily threatens the mathematical assumptions behind Bitcoin’s elliptic-curve signatures. Grover’s algorithm concerns generic search against hash functions. These are different threats with different consequences.

## 3. The UTXO model and transaction authorization

Bitcoin does not store balances in accounts. Instead, it tracks **Unspent Transaction Outputs**, commonly called UTXOs. Each output contains:
- an amount of bitcoin,
- and a locking condition expressed as a script or standardized output program.

A later transaction spends that output by providing data that satisfies the locking condition. For example, a common locking condition P2PKH requires:

> To spend this output, provide a public key whose hash matches the committed value and provide a valid signature under that public key.

A Bitcoin transaction generally includes:

- one or more inputs referencing previous outputs,
- one or more newly created outputs,
- amounts,
- scripts or witness programs,
- signatures or other witness data,
- and transaction-level fields such as version and lock time.

The signed transaction ensures that an attacker cannot freely modify protected transaction fields after the owner signs.

## 4. Private keys and public keys

A Bitcoin private key is a secret integer selected from a large valid range. It must remain known only to its owner. The corresponding public key is an elliptic-curve point derived from the private key.

Let:

- \(d\) be the private key,
- \(G\) be the standard generator point of the elliptic curve,
- \(P\) be the public key.

The public key is calculated as:

\[
P = dG
\]

Here, \(dG\) means scalar multiplication: adding the elliptic-curve point \(G\) to itself according to the curve’s group law \(d\) times.

Computing \(P\) from \(d\) is efficient. Reversing that operation that means recovering \(d\) from \(P\) is believed to be computationally infeasible for classical computers when the parameters are chosen correctly. The difficulty of recovering \(d\) from \(P=dG\) is called the **Elliptic-Curve Discrete Logarithm Problem**, or ECDLP.

Bitcoin’s current signature schemes: ECDSA and Schnorr signatures, both depend on the practical hardness of this problem.

## 5. The secp256k1 elliptic curve

Bitcoin uses an elliptic curve called **secp256k1**. Its points satisfy the equation:

\[
y^2 = x^3 + 7 \pmod p
\]

where \(p\) is a large prime defining the finite field over which the curve operates.

The curve also defines:

- a generator point \(G\),
- a large prime group order \(n\),
- rules for point addition,
- and rules for scalar multiplication.

A valid private key is an integer in the range:

\[
1 \leq d < n
\]

The corresponding public key is:

\[
P=dG
\]

The term “256” refers to the approximate bit size of the field and group parameters. This does not mean that every relevant attack requires exactly \(2^{256}\) operations. Classical generic attacks against the discrete logarithm problem have a square-root structure, giving secp256k1 an intended classical security level of approximately 128 bits.

### 5.1 Public-key encodings

An elliptic-curve public key is a point with \(x\) and \(y\) coordinates.

Traditional Bitcoin public keys may be encoded as:

- an uncompressed public key containing both coordinates,
- or a compressed public key containing the \(x\)-coordinate plus one bit identifying which \(y\)-coordinate is used.

Compressed public keys are 33 bytes.

BIP340 Schnorr signatures use **x-only public keys**. An x-only public key stores the 32-byte \(x\)-coordinate and uses a convention that selects the curve point with an even \(y\)-coordinate. This reduces public-key size without weakening the discrete-log security assumption.

## 6. Digital signatures

A digital signature scheme usually provides three operations:

1. **Key generation:** create a private key and corresponding public key.
2. **Signing:** use the private key to sign a message.
3. **Verification:** use the public key to verify the signature.

A secure digital signature should prevent an attacker from creating a valid signature without the private key. A valid signature demonstrates that the signer authorized the transaction data covered by the transaction digest. A signature does not encrypt the transaction. Bitcoin transactions remain publicly visible. The signature proves authorization, it does not provide confidentiality.

## 7. ECDSA in Bitcoin

Bitcoin originally adopted the **Elliptic Curve Digital Signature Algorithm**, or ECDSA, over secp256k1. ECDSA remains widely used for non-Taproot Bitcoin transaction outputs.

### 7.1 Simplified ECDSA signing

Let:

- \(d\) be the private key,
- \(P=dG\) be the public key,
- \(z\) be the transaction message digest,
- \(k\) be a secret per-signature nonce.

The signer computes:

\[
R=kG
\]

and derives:

\[
r=x(R)\bmod n
\]

The second signature component is:

\[
s=k^{-1}(z+rd)\bmod n
\]

The ECDSA signature is the pair:

\[
(r,s)
\]

The verifier uses the public key, message digest and signature to reconstruct a curve point and checks whether its \(x\)-coordinate is consistent with \(r\).

### 7.2 Why the nonce matters

The nonce \(k\) must not be reused. If two different messages are signed with the same nonce, an observer may be able to recover the private key algebraically. Biased, predictable, or partially leaked nonces can also compromise private keys. For this reason, secure implementations derive or generate nonces carefully and protect the signing operation against side-channel and fault-injection attacks.

### 7.3 ECDSA encoding

Bitcoin ECDSA signatures are traditionally encoded using Distinguished Encoding Rules (DER). Bitcoin consensus and policy rules also impose requirements intended to reduce signature malleability.

### 7.4 ECDSA and public-key exposure

ECDSA verification requires a public key. Depending on the Bitcoin output type, the public key may be:

- visible directly in the output,
- hidden behind a hash until spending,
- or contained inside a script that is revealed only when a particular spending path is used.

This difference is central to later post-quantum risk analysis.

## 8. Schnorr signatures in Bitcoin

Taproot introduced Schnorr signatures standardized by BIP340 which uses:

- secp256k1 curve,
- 32-byte x-only public keys,
- fixed-size 64-byte signatures,
- tagged hashes,
- and a fully specified verification algorithm.

Schnorr signatures rely on the same elliptic-curve discrete logarithm assumption as ECDSA. Its advantages are in simplicity, security analysis, linearity, compact encoding and support for higher-level protocols.

### 8.1 Simplified Schnorr signing

Let:

- \(d\) be the private key,
- \(P=dG\) be the public key,
- \(k\) be a signing nonce,
- \(R=kG\) be the nonce point,
- \(m\) be the message.

The signer calculates a challenge:

\[
e=H_{\mathrm{tag}}(R \parallel P \parallel m)
\]

and computes:

\[
s=k+ed \pmod n
\]

The signature consists of:

\[
(R,s)
\]

In BIP340 encoding, the signature stores the x-coordinate of \(R\) and the scalar \(s\).

The verifier checks:

\[
sG=R+eP
\]

### 8.2 Linearity

Schnorr signatures interact naturally with key aggregation, multisignature, threshold-signature and adaptor-signature protocols. Multiple participants can cooperate to produce one signature that verifies against an aggregate public key. From the blockchain’s perspective, such a signature can look like an ordinary single-party signature. Examples of constructions built around Schnorr’s linear structure include:

- MuSig2 multisignatures,
- threshold Schnorr protocols,
- adaptor signatures,
- and some privacy-preserving contract constructions.

### 8.3 Tagged hashes

BIP340 uses tagged hashes for domain separation. A tagged hash binds a hash operation to a specific context.

H_(x)
=
SHA256(
SHA256(tag)
||
SHA256(tag)
||
x
)

Different tags are used for different purposes, such as nonce derivation and signature challenges. Domain separation reduces the risk that data created for one cryptographic context is accidentally interpreted as valid data in another context.


## 9. Comparison of ECDSA and Schnorr

| Property | ECDSA | BIP340 Schnorr |
|---|---|---|
| Curve | secp256k1 | secp256k1 |
| Core security assumption | ECDLP hardness | ECDLP hardness |
| Public-key format | 33-byte compressed key | 32-byte x-only key |
| Signature encoding | Variable-length DER | Fixed 64-byte encoding |
| Linear structure | Not linear | Linear |
| Batch verification | Not supported | Supported |
| Key aggregation | Complex | Implemented in MuSig-style multisignature protocols |
| Post-quantum secure | No | No |

From a post-quantum perspective, the central question is whether a future attacker can solve the discrete logarithm problem for an exposed secp256k1 public key.

## 10. Hash functions in Bitcoin

A cryptographic hash function maps an input of arbitrary length to a fixed-length output. A secure hash function is expected to provide several distinct properties.

### 10.1 Preimage resistance

Given a hash value \(h\), it should be computationally infeasible to find an input \(x\) such that:

\[
H(x)=h
\]

### 10.2 Second-preimage resistance

Given one input \(x\), it should be infeasible to find another input \(x'\neq x\) such that:

\[
H(x')=H(x)
\]

### 10.3 Collision resistance

It should be infeasible to find any two distinct inputs \(x\) and \(x'\) such that:

\[
H(x)=H(x')
\]

## 11. Hash functions used by Bitcoin

Bitcoin uses following hash functions in several contexts.

### 11.1 SHA-256

SHA-256 produces a 256-bit digest. It is used directly or as part of larger constructions for:

- proof of work,
- transaction and block identifiers,
- Merkle trees,
- witness commitments,
- scripts,
- Taproot,
- Schnorr signatures,
- and key or script commitments.

### 11.2 Double SHA-256

Many traditional Bitcoin identifiers use SHA-256 twice:

SHA256d(x)
=
SHA256(SHA256(x))

Examples include:

- block-header hashes,
- traditional transaction identifiers,
- and several internal serialization checks.

### 11.3 HASH160

HASH160 combines SHA-256 and RIPEMD-160 hash functions:

HASH160(x)
=
RIPEMD160(SHA256(x))

It produces a 160-bit result. HASH160 is used in constructions such as:

- pay-to-public-key-hash,
- pay-to-script-hash,
- and some Bitcoin Script operations.

### 11.4 Transaction identifiers

A transaction identifier, or txid, commits to the non-witness serialization of a transaction. For SegWit transactions, the witness transaction identifier, or wtxid, also commits to witness data. This separation helped address transaction malleability and allowed signatures and other witness elements to be handled separately from the traditional transaction identifier.

### 11.5 Proof of work

Bitcoin miners repeatedly hash block headers while searching for an output numerically below the current target. This use of hashing is fundamentally different from transaction signing. A valid proof-of-work hash does not authorize spending. It demonstrates that computational work was performed while proposing a block.

## 12. Commitments

A cryptographic commitment allows a party to commit to some data and reveal or prove information about it later.

- the commitment binds the creator to the enclosed value,
- while the contents may remain hidden until opened.

Cryptographic commitments are usually expected to provide:

- **binding:** the committer should not be able to open the commitment to a different value;
- **hiding:** an observer should not learn the committed value before it is revealed.

A plain hash may act as a commitment when the committed data has enough unpredictability. However, hashing low-entropy data does not automatically hide it. An attacker may guess possible values and compare their hashes. A commitment is also not encryption. It does not necessarily allow a designated recipient to recover hidden data.

## 13. Merkle trees

A Merkle tree creates a compact commitment to many pieces of data. The leaves contain hashes of individual elements. Pairs of hashes are combined repeatedly until one root remains. The root commits to the complete collection. A Merkle proof allows someone to prove that one element belongs to the committed collection without revealing or transmitting every other element. Bitcoin uses Merkle constructions in several places:

- block transaction Merkle roots,
- witness commitments,
- Taproot script trees,
- and other authenticated data structures.

## 14. Bitcoin Script

Bitcoin Script is the validation language used to express many spending conditions. It is stack-based and intentionally constrained. It supports operations for:

- signature verification,
- hash comparison,
- equality checks,
- timelocks,
- conditional execution,
- and limited arithmetic and stack manipulation.

An output contains or commits to a spending condition. An input provides the data required to satisfy it. Depending on the output type, spending data may appear in:

- the `scriptSig`,
- the witness,
- a redeem script,
- a witness script,
- or a Taproot script-path witness.

## 15. Common output patterns

The following overview introduces common output types.

### 15.1 Pay to Public Key

A pay-to-public-key output places the public key directly in the locking script. The spender provides a valid signature. Because the public key is visible immediately, this type has a different long-term exposure profile from hash-based outputs.

### 15.2 Pay to Public Key Hash

A pay-to-public-key-hash output commits to a HASH160 value derived from a public key. Before spending, the output reveals the public-key hash rather than the full public key. To spend, the input reveals:

- the public key,
- and a valid signature.

Once spent, the public key becomes permanently visible on the blockchain.

### 15.3 Pay to Witness Public Key Hash

P2WPKH follows a similar authorization model to P2PKH, but places the signature and public key in the SegWit witness. The public key remains hidden behind its hash until spending, assuming the same key has not already been exposed elsewhere.

### 15.4 Pay to Script Hash and Pay to Witness Script Hash

P2SH and P2WSH commit to scripts rather than directly to one public key. The full spending script is generally revealed when the output is spent. The public-key exposure therefore depends on the script itself. A script may contain:

- one public key,
- multiple public keys,
- hashlocks,
- timelocks,
- or combinations of conditions.

### 15.5 Pay to Taproot

P2TR commits to a 32-byte x-only secp256k1 output key. A Taproot output may be spent using:

- the key path,
- or a script path.

Because the output key is visible from output creation, Taproot has a distinct public-key exposure profile that is important for post-quantum analysis.

## 16. Taproot

Taproot combines Schnorr signatures, key tweaking, and Merkleized scripts. Its main goals include:

- improving privacy,
- reducing the on-chain cost of unused script branches,
- making complex policies look similar to ordinary single-key spends,
- and enabling more flexible higher-level protocols.

### 16.1 Internal key and output key

A Taproot construction begins with an internal public key \(P\). If scripts are included, they are committed to using a Merkle root \(m\). A tweak is derived from the internal key and the script-tree commitment. Conceptually, the output key is:

\[
Q=P+H_{\text{TapTweak}}(P\parallel m)G
\]

The blockchain output commits to \(Q\), not directly to every possible script.

### 16.2 Key-path spending

In a key-path spend, the spender produces a valid BIP340 Schnorr signature for the Taproot output key. No script or Merkle proof needs to be revealed.

### 16.3 Script-path spending

In a script-path spend, the spender reveals:

- the executed script,
- the data satisfying that script,
- and a control block proving that the script belongs to the committed Taproot tree.

Only the selected script branch is revealed. Unused branches remain hidden.

### 16.4 Tapscript

BIP342 defines the script-validation rules used for Taproot script-path spending. Tapscript updates several script rules and uses BIP340 Schnorr signatures for signature checks. Taproot’s distinction between key-path and script-path spending is especially important for post-quantum readiness:

- the output key is visible from output creation,
- script-contained public keys may remain hidden until a script path is used,
- and different migration proposals may preserve, remove, disable, or reinterpret these paths differently.

## 17. Multisignatures and threshold signatures

Bitcoin can require multiple parties to cooperate before coins are spent. There are several ways to represent this.

### 17.1 Script-based multisignature

A script can list multiple public keys and require a threshold number of valid signatures. The spending policy may become visible when the script is revealed. This can disclose:

- the number of participants,
- the threshold,
- the public keys,
- and the exact policy structure.

### 17.2 Schnorr key aggregation

Protocols such as MuSig2 allow several participants to combine their keys and collaboratively produce one BIP340-compatible Schnorr signature. To an on-chain verifier, the result can resemble a normal single-key signature. This can improve:

- privacy,
- witness size,
- and on-chain efficiency.

### 17.3 Threshold signatures

A threshold signature system distributes control of one logical signing key across multiple participants. For example, a \(t\)-of-\(n\) threshold scheme allows any authorized subset of at least \(t\) participants to sign, while fewer than \(t\) cannot. Threshold signatures differ from script-based multisignature. In a threshold scheme, the blockchain may see one public key and one signature rather than the full participant policy. Multiparty signing protocols require careful treatment of:

- nonce generation,
- malicious signers,
- aborts,
- key generation,
- participant authentication,
- and transcript validation.

## 18. Connection to post-quantum readiness

The cryptographic concepts in this chapter support several important distinctions.

### 18.1 Public keys and public-key hashes are different

An elliptic-curve public key is the input needed for signature verification and the object targeted by a discrete-log attack. A public-key hash is a digest of that public key. An output that contains a public-key hash may delay exposure of the full public key until spending, assuming the key has not been revealed elsewhere.

### 18.2 Signatures and hashes face different quantum threats

ECDSA and Schnorr depend on elliptic-curve discrete logarithm hardness. SHA-256 and HASH160 depend on hash-function security properties. Shor’s and Grover’s algorithms affect these assumptions differently.

### 18.3 Output design affects exposure time

P2PK, P2PKH, P2WPKH, script-based outputs, and P2TR reveal different cryptographic information at different times. This affects whether an attacker has:

- a long period to attack a visible public key,
- or only a short window after a spending transaction is broadcast.

### 18.4 Taproot introduces two spending paths

Taproot key-path and script-path spending have different disclosure properties. This distinction matters for proposals such as Pay-to-Merkle-Root or migration designs that rely on script-only spending.

### 18.5 A post-quantum signature is not enough by itself

Even after identifying a quantum-resistant signature scheme, Bitcoin would still need to decide:

- how outputs commit to PQ keys or scripts,
- how transaction weight is accounted for,
- how verification cost is limited,
- how wallets migrate existing coins,
- how lost or dormant coins are handled,
- and how new consensus rules are activated.

## 19. Summary

Bitcoin’s cryptographic architecture is composed of several separate but interacting mechanisms:

- Private keys authorize control.
- Public keys enable signature verification.
- ECDSA and Schnorr authorize spending.
- secp256k1 provides the elliptic-curve group used by both schemes.
- Hash functions provide identifiers, proof-of-work, commitments, and script primitives.
- Merkle trees commit efficiently to collections of data.
- Bitcoin Script defines spending conditions.
- SegWit separates witness data from the traditional transaction identifier.
- Taproot combines Schnorr signatures, key tweaking, and Merkleized scripts.
- Output types determine when public keys and scripts become visible.


## 20. Further reading

For primary specifications, introductory resources, and implementation references, check:

- BIP340: Schnorr Signatures for secp256k1
- BIP341: Taproot: SegWit version 1 spending rules
- BIP342: Validation of Taproot Scripts
- Bitcoin developer documentation on transactions and wallets
- Bitcoin Core’s `libsecp256k1` implementation
