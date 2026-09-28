# UTXO Exposure Classification Model

> **Status:** Draft specification  
> **Version:** `0.1`  
> **Scope:** Educational classification of Bitcoin public-key exposure

## 1. Purpose

This document defines the classification model used by the UTXO Exposure Classifier. The model determines when a Bitcoin UTXO exposes a complete elliptic-curve public key or script containing such a key. It focuses on the information available to a hypothetical future attacker capable of recovering secp256k1 private keys from exposed public keys.

## 2. Threat model

The model assumes a hypothetical attacker who can:

1. observe blockchain data;
2. observe selected pending transactions;
3. obtain some externally supplied wallet metadata;
4. identify complete secp256k1 public keys;
5. use a sufficiently capable quantum computer to recover a private key from a public key;
6. and create an otherwise valid Bitcoin spending transaction.

The attacker does not automatically know:

- a hidden public key preimage;
- an unrevealed redeem script;
- an unrevealed witness script;
- an unrevealed Taproot script leaf;
- a private wallet descriptor;
- or a private extended public key.

The model separately records evidence that such data has been revealed.

## 3. Classification subject

The primary classification subject is one Bitcoin transaction output identified by:

```text
txid:vout
```

The classifier may also consider related evidence from:

- earlier transactions;
- pending transactions;
- scripts;
- public keys;
- descriptors;
- extended public keys;
- and wallet metadata.

The classification applies to the output’s **exposure state**, not its ownership, current market value or overall security.

## 4. Core terms

### Public key

A complete secp256k1 point sufficient for ECDSA or BIP340 Schnorr verification. Examples include:

- compressed public key;
- uncompressed public key;
- x-only BIP340 public key;
- Taproot output key.

### Public-key hash

A hash commitment to a public key, such as:

```text
HASH160(public key)
```

A public-key hash is not treated as direct public-key exposure.

### Script commitment

A hash or Merkle-root commitment to a script or script tree. Examples include:

- P2SH redeem-script hash;
- P2WSH witness-script hash;
- Taproot script-tree commitment;
- draft P2MR script-tree root.

### Long exposure

A complete vulnerable public key is visible before the legitimate owner begins spending the output.

### Short exposure

A previously hidden key becomes visible through a pending spending transaction.

### Reuse exposure

A key or script controlling an unspent output has already been disclosed through another transaction or use.

### Off-chain exposure

A key is available through information not directly contained in the UTXO, such as an xpub or wallet descriptor.

## 5. Primary exposure states

Exactly one primary state should be returned for each classification.

### `EXPOSED_AT_CREATION`

Use when the output itself contains a complete elliptic-curve public key. Typical examples:

- P2PK;
- bare multisignature;
- P2TR output key.

Properties:

```text
complete EC key visible: yes
long-exposure window: active from output creation
direct Shor target: yes, under the assumed future threat model
```

### `HIDDEN_UNTIL_SPEND`

Use when the output contains a public-key hash and no evidence shows that the complete key has already been revealed. Typical examples:

- fresh P2PKH;
- fresh P2WPKH;
- fresh P2SH-P2WPKH.

Properties:

```text
complete EC key visible: no
expected reveal event: spending transaction
short-exposure potential: yes
```

### `SCRIPT_DEPENDENT_HIDDEN`

Use when the output contains a script commitment and the script has not been revealed or supplied to the classifier. Typical examples:

- fresh P2SH;
- fresh P2WSH;
- fresh P2SH-P2WSH.

Properties:

```text
complete EC key visible: unknown
script visible: no
classification depends on hidden script
```

This state does not imply that the script is quantum-safe. It indicates insufficient visibility into the script’s authorization conditions.

### `EXPOSED_BY_REUSE`

Use when the relevant key or script was initially hidden but has already been revealed through another transaction or repeated use. Typical examples:

- P2PKH key revealed in an earlier spend;
- P2WPKH address reused after one output was spent;
- P2WSH witness script revealed by a previous spend;
- repeated Taproot script leaf containing the same public keys.

Properties:

```text
complete EC key visible: yes, where the reused script contains such keys
long-exposure window: active
exposure source: prior transaction or reuse
```

### `EXPOSED_OFFCHAIN`

Use when the key is not directly exposed by the output or prior on-chain use but is known through external information. Typical evidence:

- public xpub;
- leaked or intentionally shared descriptor;
- watch-only wallet export;
- published public key;
- external protocol reusing the key.

Properties:

```text
on-chain appearance: may remain hash-committed
actual public-key availability: yes
long-exposure window: active from external disclosure
```

### `SHORT_EXPOSURE_ACTIVE`

Use when a pending transaction has revealed a public key or script that was previously hidden. Typical examples:

- unconfirmed P2PKH spend;
- unconfirmed P2WPKH spend;
- unconfirmed P2WSH spend revealing an EC-key script;
- unconfirmed draft P2MR spend revealing a classical-signature leaf.

Properties:

```text
public key newly visible: yes
pending conflicting-spend window: active
exposure source: transaction broadcast
```

If the same key already controlled another output before the pending spend, those other outputs may separately be classified as `EXPOSED_BY_REUSE`.

### `NO_OUTPUT_KEY_SCRIPT_DEPENDENT`

Use when the output contains no elliptic-curve output key but commits to one or more scripts whose eventual authorization scheme determines exposure. Primary example:

- draft P2MR.

Properties:

```text
output-level EC key: no
long-exposure EC target at output layer: no
spending-time exposure: depends on selected leaf
```

This state must not be interpreted as fully post-quantum. A revealed leaf may still contain an ECDSA or Schnorr key.

### `UNKNOWN`

Use when available evidence is insufficient or contradictory. Examples:

- non-standard output not parsed by the classifier;
- unknown script commitment;
- incomplete wallet metadata;
- unclear key-reuse status;
- unsupported future output type.

The classifier should prefer `UNKNOWN` over an unsupported positive security claim.

## 6. Modifiers

Modifiers add context without replacing the primary state. Supported modifiers include:

| Modifier | Meaning |
|---|---|
| `KEY_REUSED` | The same key controls or controlled more than one output. |
| `SCRIPT_REUSED` | The same script commitment appears in multiple outputs. |
| `XPUB_EXPOSED` | An extended public key exposes or derives the relevant public key. |
| `DESCRIPTOR_EXPOSED` | A descriptor reveals relevant key or policy information. |
| `PENDING_SPEND` | An unconfirmed transaction is currently spending the output. |
| `TAPROOT_OUTPUT_KEY` | The output exposes a P2TR x-only output key. |
| `TAPROOT_SCRIPT_PATH_REVEALED` | A Taproot script-path spend disclosed a selected leaf and control block. |
| `THRESHOLD_POLICY` | Spending requires multiple signatures or a threshold. |
| `CLASSICAL_EC_AUTHORIZATION` | Spending ultimately depends on ECDSA or secp256k1 Schnorr. |
| `PQ_AUTHORIZATION` | Spending uses a specified post-quantum mechanism. |
| `DRAFT_PROPOSAL` | The classified output form is not currently deployed consensus. |
| `SCRIPT_CONTENT_UNKNOWN` | Script contents are not available. |
| `OFFCHAIN_EVIDENCE` | Classification depends on externally supplied information. |
| `LOW_CONFIDENCE` | Evidence is incomplete or weak. |

## 7. Evidence fields

The classifier should record the evidence used to reach a result. Recommended evidence fields:

```json
{
  "funding_output": [],
  "prior_transactions": [],
  "pending_transactions": [],
  "public_keys": [],
  "scripts": [],
  "xpubs": [],
  "descriptors": [],
  "external_observations": []
}
```

Evidence should distinguish:

- direct observation;
- user-supplied assertion;
- inferred relation;
- and unavailable information.

## 8. Proposed input schema

The initial manually supplied input may use the following fields:

```json
{
  "txid": "string",
  "vout": 0,
  "output_type": "P2WPKH",
  "output_status": "unspent",
  "commitment_type": "public_key_hash",
  "public_key_visible_at_creation": false,
  "public_key_known_onchain": false,
  "public_key_known_offchain": false,
  "key_reused": false,
  "script_revealed": false,
  "script_contains_ec_keys": null,
  "script_reused": false,
  "pending_spend_detected": false,
  "pending_spend_reveals_ec_key": false,
  "taproot_output_key_present": false,
  "authorization_scheme": "ECDSA",
  "threshold": "1-of-1",
  "evidence": [],
  "notes": []
}
```

## 9. Proposed output schema

```json
{
  "txid": "string",
  "vout": 0,
  "primary_state": "HIDDEN_UNTIL_SPEND",
  "exposure_window": "latent-short-exposure",
  "modifiers": [
    "CLASSICAL_EC_AUTHORIZATION"
  ],
  "confidence": "high",
  "reason": "The output contains a public-key hash and no evidence indicates prior disclosure of the complete public key.",
  "evidence_used": [],
  "limitations": [
    "Off-chain key disclosure cannot be excluded without wallet metadata."
  ]
}
```

## 10. Exposure-window values

The optional `exposure_window` field may use:

| Value | Meaning |
|---|---|
| `long-active` | A complete vulnerable key is already available. |
| `short-active` | A pending spend has newly exposed the key. |
| `latent-short-exposure` | The key is currently hidden but is expected to be revealed during spending. |
| `script-dependent` | Exposure depends on an unknown or unrevealed script. |
| `no-output-key-observed` | No elliptic-curve output key is visible, but script authorization still requires analysis. |
| `not-applicable` | The classified authorization does not use an elliptic-curve key. |
| `unknown` | Insufficient evidence. |

## 11. Classification precedence

When several states appear applicable, use the following precedence.

### Rule 1: Output-level key exposure

If the output itself contains a complete elliptic-curve public key:

```text
primary_state = EXPOSED_AT_CREATION
```

This takes precedence over reuse and pending-spend states.

Examples:

- P2PK;
- bare multisig;
- P2TR.

Reuse and pending-spend observations may still be added as modifiers.

### Rule 2: Off-chain key disclosure

If the output does not expose the key directly but the key is known through an xpub, descriptor, or other external source:

```text
primary_state = EXPOSED_OFFCHAIN
```

### Rule 3: Prior on-chain disclosure or reuse

If the key or relevant script was previously revealed:

```text
primary_state = EXPOSED_BY_REUSE
```

### Rule 4: Active pending-spend disclosure

If the key was previously hidden and a pending transaction now reveals it:

```text
primary_state = SHORT_EXPOSURE_ACTIVE
```

### Rule 5: No output key, script-dependent authorization

If the output directly commits to a script tree without an EC output key:

```text
primary_state = NO_OUTPUT_KEY_SCRIPT_DEPENDENT
```

unless a pending spend has already revealed a vulnerable key.

### Rule 6: Public-key-hash output

If the output contains a public-key hash and no stronger exposure evidence exists:

```text
primary_state = HIDDEN_UNTIL_SPEND
```

### Rule 7: Script-hash output

If the output contains a script commitment and the script is unavailable:

```text
primary_state = SCRIPT_DEPENDENT_HIDDEN
```

### Rule 8: Insufficient information

Otherwise:

```text
primary_state = UNKNOWN
```

## 12. Decision procedure

The initial classifier can implement the following pseudocode:

```text
function classify(record):

    modifiers = derive_modifiers(record)

    if record.public_key_visible_at_creation == true:
        return EXPOSED_AT_CREATION

    if record.public_key_known_offchain == true:
        return EXPOSED_OFFCHAIN

    if record.public_key_known_onchain == true
       or record.key_reused == true
       or exposed_reused_script(record):
        return EXPOSED_BY_REUSE

    if record.pending_spend_detected == true
       and record.pending_spend_reveals_ec_key == true:
        return SHORT_EXPOSURE_ACTIVE

    if record.output_type == P2MR
       or record.commitment_type == script_tree_root_without_output_key:
        return NO_OUTPUT_KEY_SCRIPT_DEPENDENT

    if record.commitment_type == public_key_hash:
        return HIDDEN_UNTIL_SPEND

    if record.commitment_type == script_hash:
        return SCRIPT_DEPENDENT_HIDDEN

    return UNKNOWN
```

## 13. Confidence levels

### High confidence

Use when classification follows directly from:

- the funding output;
- a decoded spending transaction;
- a known script;
- or verified public-key reuse.

### Medium confidence

Use when classification depends on:

- partial wallet metadata;
- inferred script reuse;
- incomplete transaction history;
- or a user-supplied xpub or descriptor.

### Low confidence

Use when:

- evidence is incomplete;
- the output is non-standard;
- script contents are unknown;
- or off-chain exposure cannot be verified.

A low-confidence result should normally include:

```text
LOW_CONFIDENCE
```

as a modifier.

## 14. Required test cases

The first implementation should include at least the following cases.

### Case 1: P2PK

Expected result:

```text
EXPOSED_AT_CREATION
```

### Case 2: Fresh P2PKH

Expected result:

```text
HIDDEN_UNTIL_SPEND
```

### Case 3: Pending P2PKH spend

Expected result:

```text
SHORT_EXPOSURE_ACTIVE
```

### Case 4: Reused P2PKH key

Expected result:

```text
EXPOSED_BY_REUSE
```

### Case 5: Fresh P2WPKH

Expected result:

```text
HIDDEN_UNTIL_SPEND
```

### Case 6: Public xpub deriving a P2WPKH key

Expected result:

```text
EXPOSED_OFFCHAIN
```

### Case 7: Fresh P2WSH with unknown script

Expected result:

```text
SCRIPT_DEPENDENT_HIDDEN
```

### Case 8: Revealed P2WSH multisig script

Expected result:

```text
EXPOSED_BY_REUSE
```

with:

```text
THRESHOLD_POLICY
SCRIPT_REUSED
CLASSICAL_EC_AUTHORIZATION
```

where applicable.

### Case 9: P2TR

Expected result:

```text
EXPOSED_AT_CREATION
```

with:

```text
TAPROOT_OUTPUT_KEY
CLASSICAL_EC_AUTHORIZATION
```

### Case 10: P2TR using a NUMS internal key

Expected result:

```text
EXPOSED_AT_CREATION
```

The internal-key construction does not change the exposure of the final output key.

### Case 11: Draft P2MR with hidden Schnorr leaf

Expected result:

```text
NO_OUTPUT_KEY_SCRIPT_DEPENDENT
```

with:

```text
DRAFT_PROPOSAL
CLASSICAL_EC_AUTHORIZATION
```

### Case 12: Pending draft P2MR spend revealing Schnorr key

Expected result:

```text
SHORT_EXPOSURE_ACTIVE
```

with:

```text
DRAFT_PROPOSAL
PENDING_SPEND
CLASSICAL_EC_AUTHORIZATION
```

## 15. Future extensions

Possible future versions may include:

- automatic raw-transaction decoding;
- Bitcoin Core RPC integration;
- on-chain key-reuse detection;
- script-reuse detection;
- Taproot script-path parsing;
- wallet descriptor support;
- batch UTXO classification;
- exposure-transition history;
- confidence scoring;
- and machine-readable reports.

Any future numerical risk score must be designed carefully and must not imply a precise probability of quantum theft.
