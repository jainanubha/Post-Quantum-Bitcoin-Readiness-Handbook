# UTXO Exposure Classifier

> **Status:** Design and early prototype  
> **Version:** `v0.1-dev`  
> **Purpose:** Educational analysis of Bitcoin public-key exposure under a post-quantum threat model

## Overview

The UTXO Exposure Classifier is an educational developer lab and its purpose is to classify Bitcoin transaction outputs according to when their elliptic-curve public keys or spending scripts become visible to an observer. The classifier is intended to help Bitcoin developers, educators, wallet builders and researchers reason about:

- public keys exposed directly in transaction outputs;
- public keys hidden behind hashes until spending;
- script-hash outputs whose exposure depends on the committed script;
- Taproot output-key exposure;
- key and script reuse;
- public-key disclosure through pending transactions;
- off-chain exposure through xpubs or wallet descriptors;
- and the distinction between long-exposure and short-exposure quantum attacks.

This lab does not attempt to predict when a practical quantum attack will become possible. It models the information available to a hypothetical attacker once such an attack is feasible.

## Project goals

The first version of the classifier aims to:

1. Define a consistent public-key exposure taxonomy.
2. Classify common Bitcoin output types using that taxonomy.
3. Explain why an output received a particular classification.
4. Distinguish output structure from wallet behaviour.
5. Represent uncertainty explicitly.
6. Provide deterministic test cases for future implementation.
7. Support the handbook’s address-exposure matrix and regtest labs.

## What this tool classifies

The classifier considers the following output types:

- Pay to Public Key (`P2PK`);
- bare multisignature;
- Pay to Public Key Hash (`P2PKH`);
- Pay to Script Hash (`P2SH`);
- Pay to Witness Public Key Hash (`P2WPKH`);
- Pay to Witness Script Hash (`P2WSH`);
- nested SegWit outputs;
- Pay to Taproot (`P2TR`);
- and the draft Pay-to-Merkle-Root (`P2MR`) proposal.

The primary exposure states are:

| State | Meaning |
|---|---|
| `EXPOSED_AT_CREATION` | A complete elliptic-curve public key is visible in the output from creation. |
| `HIDDEN_UNTIL_SPEND` | The output commits to a public-key hash and normally reveals the key only during spending. |
| `SCRIPT_DEPENDENT_HIDDEN` | The output commits to a script whose public-key exposure is not yet known. |
| `EXPOSED_BY_REUSE` | A previously hidden key or script has already been revealed through reuse or another transaction. |
| `EXPOSED_OFFCHAIN` | The key is not visible in the output but is known through an xpub, descriptor, export, or other external source. |
| `SHORT_EXPOSURE_ACTIVE` | A pending transaction has revealed a previously hidden public key or script. |
| `NO_OUTPUT_KEY_SCRIPT_DEPENDENT` | No elliptic-curve output key is visible, but the spending script may still use vulnerable signatures. |
| `UNKNOWN` | Available evidence is insufficient for a reliable classification. |

An output classified as `HIDDEN_UNTIL_SPEND` may still be vulnerable to:

- seed theft;
- malware;
- weak key generation;
- compromised backups;
- phishing;
- implementation bugs;
- or other classical attacks.

Similarly, an output classified as `EXPOSED_AT_CREATION` is not presently broken. It indicates that the complete elliptic-curve public key is available to a hypothetical future attacker capable of solving the secp256k1 discrete logarithm problem.

This lab implements concepts introduced in:

- [`Bitcoin Address Types and Public-Key Exposure`](../../handbook/06-bitcoin-address-types-and-public-key-exposure.md)
- [`Long-Exposure versus Short-Exposure Quantum Attacks`](../../handbook/07-long-exposure-vs-short-exposure-attacks.md)
- [`Taproot Key-Path and Script-Path Considerations`](../../handbook/08-taproot-key-path-and-script-path-considerations.md)
- [`Bitcoin Address-Type Exposure Matrix`](../../tables/address-type-exposure-matrix.md)

## Proposed input model

The initial prototype will accept manually supplied structured input rather than scanning Bitcoin Core, a wallet, or the blockchain. Example input:

```json
{
  "output_type": "P2WPKH",
  "output_status": "unspent",
  "public_key_visible_at_creation": false,
  "public_key_known_onchain": false,
  "public_key_known_offchain": false,
  "key_reused": false,
  "script_revealed": false,
  "script_reused": false,
  "pending_spend_detected": false,
  "pending_spend_reveals_ec_key": false,
  "authorization_scheme": "ECDSA",
  "evidence": [
    "Funding output contains a version-0 20-byte witness program"
  ]
}
```

## Proposed output model

Example output:

```json
{
  "primary_state": "HIDDEN_UNTIL_SPEND",
  "exposure_window": "latent-short-exposure",
  "modifiers": [
    "CLASSICAL_EC_AUTHORIZATION"
  ],
  "confidence": "high",
  "reason": "The output contains a public-key hash and no evidence shows that the corresponding public key has already been revealed.",
  "limitations": [
    "Off-chain xpub or descriptor exposure cannot be ruled out from blockchain data alone."
  ]
}
```

Every output should contain:

- a primary exposure state;
- relevant modifiers;
- a confidence level;
- a human-readable explanation;
- supporting evidence;
- and known limitations.

## Development phases

### Phase 1: Exposure model

Deliverables:

- exposure-state definitions;
- classification precedence;
- input and output schemas;
- worked examples;
- limitations;
- and test vectors.

### Phase 2: Minimal classifier prototype

Deliverables:

- a small Python command-line program;
- manual JSON input;
- deterministic rule evaluation;
- human-readable and JSON output;
- and unit tests.

### Phase 3: Regtest integration

Deliverables:

- extract transaction data from Bitcoin Core regtest;
- classify selected P2PKH, P2WPKH, P2WSH, and P2TR examples;
- compare classification before and after spending;
- and record exposure transitions.

### Phase 4: Extended analysis

Possible future work:

- raw transaction parsing;
- script parsing;
- detection of on-chain key reuse;
- detection of script reuse;
- descriptor-aware analysis;
- wallet integration;
- batch UTXO reports;
- and visualization of exposure transitions.

## Contributing

Contributions are welcome in the form of:

- corrections to classification rules;
- new test cases;
- additional output types;
- regtest examples;
- script-parsing improvements;
- documentation;
- and technical review.
