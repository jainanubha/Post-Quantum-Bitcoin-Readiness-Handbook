# Grover’s Algorithm and Bitcoin Hashes

Bitcoin uses cryptographic hash functions throughout its design. Hashes are used in proof of work, transaction identifiers, block identifiers, public-key hashes, script hashes, Merkle trees, Taproot commitments, hashlocks and several signature-related constructions.

A future quantum computer would affect these uses differently from the way it would affect Bitcoin’s elliptic-curve signatures. Shor’s algorithm attacks the algebraic structure underlying ECDSA and Schnorr signatures and can, in principle, recover a private key from a public key. Grover’s algorithm does not provide the same kind of structural break against hash functions, it actually gives a quadratic speedup for certain brute-force search problems. In simplified terms:

```text
Classical exhaustive search:
N evaluations

Grover search:
approximately √N quantum oracle evaluations
```

For an ideal \(n\)-bit hash function, this changes the generic cost of a preimage attack from approximately:

\[
2^n
\]

classical evaluations to approximately:

\[
2^{n/2}
\]

quantum oracle evaluations.

This is a significant reduction in the theoretical security exponent, but it is not equivalent to the polynomial-time break that Shor’s algorithm provides against elliptic-curve signatures. A Grover-based attacker must still perform an extremely large coherent computation, and each quantum oracle evaluation may be much more expensive than one ordinary hardware hash operation. 

This chapter explains:

- how Grover’s algorithm works at a high level,
- which hash-security properties it affects,
- how Bitcoin uses SHA-256 and HASH160,
- what Grover’s algorithm means for Bitcoin mining,
- why mining is not immediately broken,
- how public-key hashes, script hashes, hashlocks and Merkle commitments are affected,
- and how Bitcoin readiness efforts should account for quantum search.

Grover’s original result shows that an unstructured search over \(N\) possibilities can be performed using \(O(\sqrt{N})\) quantum steps rather than \(O(N)\) classical steps.

## 1. Learning objectives

After reading this chapter, a reader should be able to explain:

1. What problem Grover’s algorithm solves.
2. Why Grover’s algorithm is different from Shor’s algorithm.
3. What an oracle means in the context of quantum search.
4. Why Grover provides a quadratic rather than exponential speedup.
5. How Grover affects preimage and second-preimage security.
6. Why collision security follows a different quantum complexity.
7. How Bitcoin uses SHA-256, double SHA-256, and HASH160.
8. What Grover’s algorithm means for Bitcoin proof of work.
9. Why a Grover-based mining advantage does not immediately break Bitcoin consensus.
10. Why HASH160 outputs have a different quantum-security margin from SHA-256 outputs.
11. Why finding an arbitrary collision is not the same as finding a spendable Bitcoin preimage.
12. Why practical quantum resource costs matter in addition to asymptotic query complexity.

## 2. Cryptographic hash properties

Before examining Grover’s algorithm, it is important to distinguish the security properties expected from a hash function. Let:

\[
H:\{0,1\}^{*}\rightarrow\{0,1\}^{n}
\]

be a hash function producing an \(n\)-bit output. NIST’s Secure Hash Standard defines SHA-256 as part of the SHA-2 family and specifies the generation of fixed-length message digests used to detect changes in messages.

### 2.1 Preimage resistance

Given a target hash value \(y\), find any input \(x\) such that:

\[
H(x)=y.
\]

For an ideal \(n\)-bit hash function, a classical brute-force preimage attack requires approximately:

\[
2^n
\]

evaluations. Grover reduces the idealized quantum query complexity to approximately:

\[
2^{n/2}.
\]

### 2.2 Second-preimage resistance

Given a particular input \(x\), find another input \(x'\neq x\) such that:

\[
H(x')=H(x).
\]

For an ideal \(n\)-bit hash function, the generic classical cost is approximately:

\[
2^n.
\]

A generic Grover-style quantum search can reduce this to approximately:

\[
2^{n/2}.
\]

### 2.3 Collision resistance

Find any two distinct inputs \(x\neq x'\) such that:

\[
H(x)=H(x').
\]

The classical birthday bound gives an expected cost of approximately:

\[
2^{n/2}.
\]

Quantum collision-finding algorithms can improve the generic query complexity further. The Brassard–Høyer–Tapp algorithm finds collisions in certain black-box functions using approximately:

\[
O(2^{n/3})
\]

queries, although it introduces important memory, implementation and model assumptions.

## 3. How Grover’s algorithm works

Grover’s algorithm begins with a search space of \(N\) possible inputs. Assume there are \(M\) valid solutions.

A quantum register is initialized in an equal superposition over possible candidates:

\[
|\psi\rangle
=
\frac{1}{\sqrt{N}}
\sum_{x=0}^{N-1}|x\rangle.
\]

An oracle marks states that satisfy the desired condition. Conceptually:

\[
O_f|x\rangle
=
(-1)^{f(x)}|x\rangle,
\]

where:

\[
f(x)=
\begin{cases}
1,&\text{if }x\text{ is a valid solution},\\
0,&\text{otherwise}.
\end{cases}
\]

The algorithm then applies an amplitude-amplification operation. Each iteration increases the probability amplitude of valid states and reduces the amplitude of invalid states. After approximately:

\[
O\left(\sqrt{\frac{N}{M}}\right)
\]

iterations, measuring the register returns a valid candidate with high probability.

For one valid answer among \(N\) possibilities, the familiar complexity is:

\[
O(\sqrt{N}).
\]

Grover’s paper presents this quadratic search advantage and notes that it is within a small constant factor of the fastest possible generic quantum search.

## 4. Quantum oracle

A Grover oracle for a Bitcoin-related search must:

1. receive a candidate input in quantum superposition,
2. compute the relevant Bitcoin function reversibly,
3. compare the result with the target condition,
4. mark valid states by changing their phase,
5. and uncompute temporary values so the next iteration can proceed.

For mining, the oracle would need a reversible implementation of the relevant block-header hashing operation. For a HASH160 address-preimage attack, the oracle may need to perform:

- secp256k1 public-key derivation,
- public-key serialization,
- SHA-256,
- RIPEMD-160,
- and comparison with the target hash.

A query count therefore does not directly translate into the same number of classical SHA-256 operations. Practical cost depends on:

- quantum circuit depth,
- logical qubit count,
- non-Clifford gate count,
- error-correction overhead,
- reversible arithmetic,
- memory,
- control operations,
- and clock speed.

This distinction is essential when interpreting statements such as:

```text
Grover reduces a 256-bit search to 128-bit quantum security.
```

That statement describes asymptotic generic query complexity. It does not prove that \(2^{128}\) fault-tolerant oracle calls are practically achievable.

## 5. Idealized classical and quantum security levels

The following table gives simplified black-box estimates.

| Hash construction | Output length | Classical preimage cost | Generic quantum preimage cost | Classical collision cost | Generic quantum collision cost |
|---|---:|---:|---:|---:|---:|
| SHA-256 | 256 bits | \(2^{256}\) | \(2^{128}\) | \(2^{128}\) | approximately \(2^{85.3}\) |
| Double SHA-256 | 256 bits | approximately \(2^{256}\) | approximately \(2^{128}\) | approximately \(2^{128}\) | approximately \(2^{85.3}\) |
| HASH160 | 160 bits | approximately \(2^{160}\) | approximately \(2^{80}\) | approximately \(2^{80}\) | approximately \(2^{53.3}\) |

These values are idealized security exponents, not complete attack-cost estimates. Important limitations include:

- actual inputs may have structure,
- a Bitcoin attacker may need a targeted preimage rather than an arbitrary collision,
- reversible oracle construction may be extremely expensive,
- memory assumptions can change quantum collision costs,
- parallelization behaves differently from classical brute force,
- and a result must still satisfy Bitcoin’s transaction or consensus rules.

### 5.1 Double hashing does not double the output length

Bitcoin often uses:

\[
\operatorname{SHA256d}(x)
=
\operatorname{SHA256}(\operatorname{SHA256}(x)).
\]

The result remains 256 bits. Applying SHA-256 twice may have design and robustness motivations, but it does not turn a 256-bit digest into a 512-bit digest. In a generic preimage-security discussion, double SHA-256 still has an output space of approximately:

\[
2^{256}.
\]

Its ideal Grover search complexity therefore remains on the order of:

\[
2^{128}
\]

oracle evaluations.

## 6. Where Bitcoin uses hashes

Bitcoin uses cryptographic hashes in several distinct contexts.

### 6.1 Proof of work

Bitcoin miners search for a block header whose hash is numerically below the current target. The Bitcoin whitepaper describes proof of work as scanning for a value whose SHA-256 hash begins with the required number of zero bits. The required average classical work grows exponentially with the number of constrained bits. Bitcoin’s implementation uses double SHA-256 for block-header hashing.

### 6.2 Transaction and block identifiers

Traditional transaction identifiers and block hashes use double SHA-256 over serialized data. These hashes act as compact identifiers and commitments to transaction or block contents.

### 6.3 Public-key hashes

P2PKH and P2WPKH use HASH160:

\[
\operatorname{HASH160}(P)
=
\operatorname{RIPEMD160}(\operatorname{SHA256}(P)).
\]

This produces a 160-bit public-key hash.

### 6.4 Script hashes

P2SH uses a 160-bit hash of a redeem script. P2WSH uses a 256-bit SHA-256 commitment to a witness script.

### 6.5 Merkle trees

Blocks commit to transactions using a Merkle root. Taproot commits to script branches using a tagged-hash-based Merkle tree.

### 6.6 Hashlocks

Bitcoin scripts can require disclosure of a value whose hash matches a committed digest. Hashlocks are used in constructions such as atomic swaps, payment channels, and other contracts.

### 6.7 Tagged hashes

BIP340 and Taproot use tagged SHA-256 constructions for domain separation. Tagged hashing ensures that data hashed for one protocol purpose is not unintentionally reused in another context.

## 7. Grover’s algorithm and Bitcoin proof of work

Bitcoin mining is a search problem. A miner constructs candidate block headers and searches for one satisfying:

\[
H(\text{header})<T,
\]

where:

- \(H\) is Bitcoin’s block-header hash function,
- and \(T\) is the current target.

If the hash behaves like a uniformly random 256-bit value, the success probability of one candidate is approximately:

\[
p=\frac{T+1}{2^{256}}.
\]

A classical miner requires approximately:

\[
\frac{1}{p}
\]

hash attempts on average. 

An ideal quantum miner using Grover-style amplitude amplification would require on the order of:

\[
\frac{1}{\sqrt{p}}
\]

oracle iterations. Equivalently, if a classical search requires approximately \(2^b\) candidate evaluations, an ideal Grover search requires approximately:

\[
2^{b/2}
\]

quantum oracle iterations. This is a quadratic advantage in query complexity.

## 8. Why Grover does not immediately break mining

Several factors separate asymptotic Grover complexity from practical Bitcoin mining.

### 8.1 The SHA-256 oracle is expensive

Every Grover iteration needs a reversible circuit implementing the relevant hash calculation and target comparison. A classical ASIC can evaluate hashes extremely efficiently using irreversible specialized hardware. A fault-tolerant quantum computer would need to implement the same logical function reversibly while maintaining coherence. The quantum miner is therefore not comparing one quantum operation with one ASIC hash.

### 8.2 Grover iterations are largely sequential

Amplitude amplification requires repeated coherent iterations. A miner cannot generally divide one Grover computation among millions of processors with the same linear scaling available to classical independent nonce search. With \(K\) quantum processors searching divided subspaces, runtime can improve approximately as:

\[
O\left(\sqrt{\frac{N}{K}}\right),
\]

which gives a square-root parallel advantage rather than a linear one.

### 8.3 Mining is a race with a deadline

Bitcoin mining is not a static search that can run indefinitely. While one miner performs a quantum search:

- another miner may find a valid block,
- the previous-block reference may change,
- transaction selection may change,
- the timestamp may change,
- and the useful search instance may need to restart.

A quantum mining strategy must fit within the competitive block-arrival process.

### 8.4 Measurement produces a candidate

Grover search is probabilistic. The miner must choose when to measure the quantum state. Measuring too early lowers the success probability. Continuing too long can rotate probability amplitude away from the solution space. The optimal iteration count depends on the estimated number of valid states.

### 8.5 Fault tolerance dominates practical design

The miner must maintain a large coherent computation with a sufficiently low logical error rate. Error correction, magic-state production, routing and reversible SHA-256 circuitry can dominate practical resource requirements.

### 8.6 Bitcoin adjusts mining difficulty

If quantum miners contributed a persistent and significant share of effective mining power, Bitcoin’s difficulty adjustment would eventually compensate by making valid block hashes harder to find. Difficulty adjustment would not remove distributional concerns. Early or concentrated quantum mining capability could still affect:

- miner centralization,
- block share,
- transaction ordering,
- censorship capability,
- and reorganization risk.

However, persistent quantum mining does not imply unlimited block production at an unchanged difficulty. Recent resource-analysis work argues that once reversible double-SHA-256 circuits, error correction, parallel quantum fleets and energy costs are included, practical quantum mining remains dramatically more difficult than the simple square-root query count suggests.

## 9. Quantum mining and consensus security

A quantum mining advantage does not automatically allow an attacker to perform every type of Bitcoin attack.

### A quantum miner may be able to:

- find valid blocks more efficiently than its classical energy cost would suggest,
- gain a larger share of block production,
- attempt chain reorganizations if it controls sufficient effective work,
- censor selected transactions,
- or influence transaction ordering.

### A quantum miner cannot automatically:

- forge ECDSA or Schnorr signatures using Grover’s algorithm,
- spend arbitrary UTXOs without satisfying their scripts,
- create bitcoin beyond the consensus issuance schedule,
- produce invalid blocks accepted by honest nodes,
- or bypass transaction-validation rules.

Mining power and signing authority remain distinct. 

## 10. Rewriting chain history

Changing an old confirmed block requires redoing that block’s proof of work and catching up with or surpassing the honest chain. A quantum attacker with a mining advantage may reduce the cost of searching for valid replacement blocks, but the attacker must still compete against ongoing honest block production. The feasibility of a reorganization depends on:

- the attacker’s effective block-production rate,
- honest network mining power,
- how far back the attacker starts,
- the time available,
- difficulty,
- and whether the attacker can sustain its advantage.

Grover’s quadratic speedup alone does not imply that a small quantum computer can instantly rewrite the blockchain.

## 11. HASH160 and Bitcoin public-key hashes

P2PKH and P2WPKH outputs commit to a 160-bit HASH160 value derived from a public key.  A fresh P2PKH-style output normally exposes:

\[
h=\operatorname{HASH160}(P)
\]

rather than the full public key \(P\). This delays a Shor attack because Shor’s elliptic-curve algorithm requires the public key itself. However, HASH160 introduces a separate theoretical Grover attack. An attacker could search for a private key \(d'\) such that its public key:

\[
P'=d'G
\]

satisfies:

\[
\operatorname{HASH160}(P')=h.
\]

This is a targeted preimage problem over a 160-bit output. Its idealized generic quantum query complexity is approximately:

\[
2^{80}.
\]

Each oracle query must include:

- generation of a candidate private scalar,
- secp256k1 scalar multiplication,
- public-key serialization,
- SHA-256,
- RIPEMD-160,
- target comparison,
- and reversible uncomputation.

The actual fault-tolerant resource cost may therefore be much larger than the simplified \(2^{80}\) exponent suggests.

## 12. Preimage attacks and collision attacks are different

A common mistake is to look at the approximate \(2^{53.3}\) quantum collision complexity of a 160-bit hash and conclude that a Bitcoin P2PKH output can be stolen with \(2^{53.3}\) work. To spend a specific P2PKH output, the attacker needs a public key whose HASH160 equals the existing target value. This is a targeted preimage problem:

\[
\operatorname{HASH160}(P')=h_{\text{target}}.
\]

Finding any two arbitrary inputs with the same HASH160 value does not satisfy the existing output. Therefore, the relevant generic quantum exponent is approximately:

\[
2^{80},
\]

not:

\[
2^{53.3}.
\]

The same distinction applies more broadly:

- arbitrary collisions may demonstrate reduced collision security,
- but many Bitcoin attacks require a preimage or second preimage for a specific existing commitment.

## 13. Public-key reveal and competing attack paths

For a fresh P2PKH or P2WPKH output, an attacker might conceptually consider two different quantum routes.

### Before the public key is revealed

The attacker may attempt a Grover preimage search against the 160-bit public-key hash. Idealized exponent:

\[
2^{80}.
\]

The oracle is complex because it includes secp256k1 public-key derivation and HASH160.

### After the public key is revealed

The attacker may attempt a Shor discrete-log attack against the exposed secp256k1 public key. The relevant attack is then private-key recovery, not hash preimage search. Which attack becomes practical first depends on:

- quantum hardware,
- circuit design,
- available runtime,
- whether the key was reused,
- and whether the attacker must race a pending transaction.

This is why public-key hashes delay one attack path but do not make the output permanently quantum-secure.

## 14. Script hashes

### 14.1 P2SH

P2SH commits to a 160-bit hash of a redeem script. An attacker trying to replace a specific redeem script with another spendable script needs a targeted preimage matching the existing P2SH hash. The ideal generic quantum preimage exponent is approximately:

\[
2^{80}.
\]

Again, the attacker must find a usable script preimage for the existing target—not merely any arbitrary hash collision.

### 14.2 P2WSH

P2WSH commits to a witness script using SHA-256. A generic preimage search against a particular P2WSH commitment has idealized quantum query complexity of approximately:

\[
2^{128}.
\]

This gives a larger generic quantum preimage margin than a 160-bit P2SH commitment.

### 14.3 Script validity still matters

Finding a byte string whose hash matches a target is insufficient unless the resulting script can be used to satisfy the output’s spending rules. The attack must produce:

- the correct hash preimage,
- a valid script,
- and witness data that makes the script evaluate successfully.

## 15. Merkle trees and quantum attacks

Bitcoin uses Merkle trees to commit efficiently to collections of data. Examples include:

- the block transaction Merkle root,
- Taproot script trees,
- witness commitments,
- and other authenticated structures.

Merkle constructions can depend on several hash properties: Collision resistance, Second-preimage resistance, Preimage resistance. However, an arbitrary collision in the underlying hash function may not directly translate into a valid Bitcoin Merkle-tree attack.

A useful attack may also need to satisfy:

- leaf encoding rules,
- branch ordering,
- tagged-hash domains,
- transaction serialization,
- script validity,
- or consensus constraints.

## 16. Taproot tagged hashes

Taproot uses tagged SHA-256 hashes for several purposes, including:

- script leaves,
- script branches,
- output-key tweaks,
- and BIP340 signature operations.

Tagged hashing provides domain separation. Conceptually:

\[
H_{\text{tag}}(x)
=
\operatorname{SHA256}
\left(
\operatorname{SHA256}(\text{tag})
\parallel
\operatorname{SHA256}(\text{tag})
\parallel
x
\right).
\]

A quantum attacker targeting a Taproot commitment still needs to solve the relevant preimage, second-preimage, or collision problem within the correct tagged domain.

## 17. Hashlocks and secret entropy

A hashlock requires a spender to reveal a secret \(s\) satisfying:

\[
H(s)=h.
\]

If \(s\) is uniformly random and has 256 bits of entropy, a classical brute-force attack requires approximately:

\[
2^{256}
\]

candidate checks, while an ideal Grover attack requires approximately:

\[
2^{128}
\]

oracle iterations. If the secret has only \(k\) bits of entropy, however, the attacker may search the actual secret space in approximately:

\[
2^{k/2}
\]

quantum queries. The security of a hashlock therefore depends not only on the digest size but also on the entropy of the secret. A 256-bit hash does not provide 128-bit quantum security when the hidden secret is a predictable word, timestamp, short PIN or low-entropy application value. Good practice is to use uniformly random, sufficiently long preimages.

## 18. Transaction identifiers and block identifiers

Bitcoin identifiers commonly use double SHA-256. Quantum attacks can theoretically reduce the generic cost of preimage search, second-preimage search and collision finding. However, changing a transaction or block while preserving a particular identifier is not simply an arbitrary hash collision problem. The replacement object must usually:

- serialize correctly,
- satisfy transaction or block structure,
- obey consensus rules,
- preserve relevant commitments,
- and produce the required target digest.

For historical chain replacement, proof-of-work must also be redone. The protocol context often makes a usable attack substantially more constrained than generic hash-search notation suggests.

## 19. Quantum parallelization

Classical brute-force searches parallelize naturally. If one classical machine tests \(r\) candidates per second, \(K\) independent machines can test approximately:

\[
Kr
\]

candidates per second.

Grover search has a different parallelization profile. If the search space is divided among \(K\) independent quantum processors, each processor searches approximately \(N/K\) candidates, requiring:

\[
O\left(\sqrt{\frac{N}{K}}\right)
\]

iterations. The runtime improvement is proportional to:

\[
\sqrt{K},
\]

rather than \(K\). Achieving another factor-of-two reduction in runtime may therefore require approximately four times as many comparable quantum processors. This square-root parallelization behavior is especially relevant to mining, where classical ASIC systems are deployed in enormous parallel fleets.

## 20. Fault-tolerant implementation costs

A practical Grover attack requires much more than a quantum register of the nominal search-space size. The attacker needs:

- logical qubits protected by error correction,
- reversible implementations of all target operations,
- ancillary qubits,
- repeated non-Clifford gates,
- reliable comparison circuits,
- uncomputation of temporary data,
- and enough coherence for the full amplitude-amplification sequence.

For SHA-256-based searches, the attacker must represent and update the SHA-256 internal state reversibly. For HASH160 public-key searches, the circuit may additionally require: 

- secp256k1 scalar multiplication,
- SHA-256,
- RIPEMD-160,
- and key-encoding logic.

A simple statement such as:

```text
SHA-256 has 128-bit quantum security
```

does not communicate this physical cost.

It is better interpreted as:

> In the generic quantum query model, Grover reduces the preimage-search exponent of an ideal 256-bit hash from 256 to 128.

## 21. Hash-based post-quantum signatures

Hash functions are also the foundation of several post-quantum signature families. Examples include:

- Lamport one-time signatures,
- Winternitz one-time signatures,
- XMSS,
- LMS,
- SPHINCS+,
- and NIST’s SLH-DSA standard.

These schemes remain viable in a quantum threat model because their parameters are selected with Grover-style and quantum collision attacks in mind. This does not mean that ordinary 256-bit hashes remain unaffected. Instead, scheme designers account for the reduced generic quantum-security margin by selecting:

- larger hash outputs,
- appropriate tree heights,
- sufficient preimage security,
- suitable few-time or one-time parameters,
- and conservative security levels.

For Bitcoin, hash-based signatures are attractive because they rely on assumptions different from elliptic-curve discrete logarithms. Their main challenges include:

- large signatures,
- large public keys,
- signing cost,
- verification cost,
- state-management requirements in some schemes,
- and block-space impact.

## 22. What Grover means for different Bitcoin components

| Bitcoin component | Relevant hash property | Idealized Grover effect | Main practical consideration |
|---|---|---|---|
| Proof of work | Threshold preimage search | Quadratic query speedup | Reversible SHA-256d, deadlines, parallelization and difficulty |
| P2PKH/P2WPKH | Targeted HASH160 preimage | \(2^{160}\rightarrow2^{80}\) queries | Oracle must derive a secp256k1 public key and HASH160 |
| P2SH | Targeted 160-bit script preimage | \(2^{160}\rightarrow2^{80}\) queries | Replacement script must be valid and spendable |
| P2WSH | Targeted SHA-256 script preimage | \(2^{256}\rightarrow2^{128}\) queries | Replacement script must satisfy consensus |
| Hashlocks | Secret preimage | Exponent approximately halved | Security limited by actual secret entropy |
| Merkle roots | Collision/second preimage | Depends on attack goal | Tree structure and encoding constrain attacks |
| Transaction IDs | Collision/second preimage | Generic exponent reduced | Replacement transaction must remain valid |
| Block hashes | Threshold/preimage search | Quadratic query speedup | Must compete with honest chain |
| Taproot commitments | Collision/second preimage | Generic exponent reduced | Tagged domains and valid tree structure matter |
| Hash-based signatures | Preimage/collision assumptions | Parameters must account for quantum attacks | Signature size and Bitcoin integration |

## 23. Readiness implications

Grover’s algorithm suggests several readiness actions.

### 23.1 Track HASH160 exposure

HASH160 has a 160-bit output, giving an idealized Grover preimage exponent of approximately 80 bits. This is lower than the approximately 128-bit generic quantum preimage margin of SHA-256. A UTXO exposure classifier should therefore distinguish:

- public-key-hash outputs,
- SHA-256 script-hash outputs,
- exposed elliptic-curve keys,
- and post-quantum outputs.

### 23.2 Avoid low-entropy hashlocks

Hashlocked secrets should be generated with high entropy. The hash output length does not compensate for a predictable secret.

### 23.3 Evaluate quantum mining realistically

Quantum mining analysis should include:

- reversible circuit cost,
- fault-tolerant logical gates,
- quantum clock speed,
- restart conditions,
- parallelization limits,
- network competition,
- and difficulty adjustment.

### 23.4 Choose adequate hash parameters for PQ constructions

Future Bitcoin post-quantum signatures and commitments must choose parameters that remain secure under Grover and quantum collision attacks.

## 24. Open questions

1. What is the most appropriate way to model a reversible double-SHA-256 mining oracle?
2. How should Bitcoin compare quantum mining capability with classical ASIC hashrate?
3. How much advantage can a quantum miner obtain under realistic block-arrival deadlines?
4. How should mining difficulty respond to concentrated quantum capability?
5. Should HASH160 outputs be assigned a different long-term readiness priority from SHA-256 script commitments?
6. What is the realistic fault-tolerant cost of a HASH160 public-key preimage attack?
7. How should secp256k1 scalar multiplication be incorporated into that oracle estimate?
8. Which Bitcoin uses depend primarily on collision resistance rather than preimage resistance?
9. How should Taproot’s tagged hashes be analyzed under quantum collision attacks?
10. What minimum entropy should Bitcoin applications require for hashlocked secrets?
11. Which hash-based post-quantum signature schemes best fit Bitcoin’s block-space constraints?
12. How should developers communicate “128-bit quantum preimage security” without implying practical attainability?
13. What regtest examples would best demonstrate the difference between a hash commitment and a revealed public key?
14. Should the UTXO exposure classifier include separate categories for 160-bit and 256-bit commitments?

## 25. Summary

Grover’s algorithm provides a quadratic speedup for unstructured search. For an ideal \(n\)-bit hash function:

\[
\text{Classical preimage cost}\approx2^n
\]

while:

\[
\text{Quantum preimage query cost}\approx2^{n/2}.
\]

For Bitcoin, this means:

- SHA-256 has an idealized generic quantum preimage margin of approximately 128 bits;
- HASH160 has an idealized generic quantum preimage margin of approximately 80 bits;
- quantum collision algorithms can reduce collision complexity further;
- but arbitrary collisions do not necessarily produce usable Bitcoin attacks.

Grover may provide a theoretical advantage in Bitcoin mining, but the practical attack requires:

- reversible double-SHA-256 circuits,
- fault-tolerant logical qubits,
- a long coherent computation,
- effective quantum parallelization,
- and completion within a competitive block-production window.

The algorithm does not automatically:

- forge signatures,
- bypass scripts,
- create invalid blocks,
- or rewrite the blockchain instantly.

Grover also creates a separate theoretical path against hash-committed outputs such as P2PKH, P2WPKH, P2SH, and P2WSH. The relevant attack is usually a targeted preimage search, not an arbitrary collision search. 

The central readiness lesson is:

> Quantum computing reduces the generic security margins of Bitcoin’s hash functions, but the effect is a quadratic search advantage whose practical significance depends heavily on circuit cost, protocol structure, and the attacker’s available time.

## 26. Further reading

For related handbook material, see:

1. Lov K. Grover, *A Fast Quantum Mechanical Algorithm for Database Search*.
2. Gilles Brassard, Peter Høyer, and Alain Tapp, *Quantum Algorithm for the Collision Problem*.
3. NIST FIPS 180-4, *Secure Hash Standard*.
4. Satoshi Nakamoto, *Bitcoin: A Peer-to-Peer Electronic Cash System*.
5. Divesh Aggarwal et al., *Quantum Attacks on Bitcoin, and How to Protect Against Them*.
6. Pierre-Luc Dallaire-Demers, *Kardashev Scale Quantum Computing for Bitcoin Mining*.
