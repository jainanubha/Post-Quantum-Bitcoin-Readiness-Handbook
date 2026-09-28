# Regtest Public-Key Exposure Lab — Implementation Plan

> **Status:** Draft implementation plan  
> **Milestone:** Month 2  
> **Target release:** Initial lab design and selected working scenarios  
> **Network:** Bitcoin Core regtest only

## 1. Objective

The objective of this lab is to create reproducible Bitcoin Core regtest examples showing when public keys and spending scripts become visible across different Bitcoin output types. The lab will compare:

- P2PKH;
- P2WPKH;
- P2WSH or another script-based output;
- P2TR key-path spending;
- and, if feasible, P2TR script-path spending.

For each output type, the lab will record:

1. what is visible when the output is created;
2. what remains hidden while it is unspent;
3. what becomes visible when the spend is broadcast;
4. what remains visible after confirmation;
5. and how the observation maps to the UTXO Exposure Classifier.

## 2. Research questions

The lab should answer the following questions.

### General

1. Does the funding output contain a complete public key, a public-key hash, or a script commitment?
2. Which transaction field contains the relevant commitment?
3. Which fields are visible to an observer without wallet-specific information?
4. Does exposure begin at broadcast or confirmation?
5. Can an unconfirmed transaction permanently reveal a key or script?
6. Does reuse of a key or script affect another UTXO?

### P2PKH and P2WPKH

1. Is the full public key visible before spending?
2. Where does the public key appear during spending?
3. Does SegWit change the exposure event or only the transaction location?
4. How does repeated use of one address affect other UTXOs?

### P2WSH

1. Is the witness script visible before spending?
2. Which public keys become visible when the witness script is revealed?
3. Can the same revealed script be linked to another unspent output?
4. How should an unknown script commitment be classified?

### P2TR

1. Is the x-only output key visible at creation?
2. What does a key-path spend reveal?
3. What does a script-path spend reveal?
4. Does a NUMS-style internal key change output-key exposure?
5. Which script keys become newly visible through script-path spending?

## 3. Technical approach

The lab should be developed in stages.

### Stage A: Documentation-first design

Create and review:

- `README.md`;
- `plan.md`;
- scenario definitions;
- observation schema;
- expected exposure transitions;
- and known limitations.

No scenario should be automated before its expected pre-spend and post-spend observations are clearly documented.

### Stage B: Single-node baseline

Use one Bitcoin Core regtest node with multiple wallets.

Suggested logical roles:

```text
miner wallet:
receives block rewards and creates spendable regtest balance

sender wallet:
funds test outputs

receiver wallet:
owns test addresses or descriptors
```

The single-node version is sufficient to inspect:

- funding outputs;
- unconfirmed spending transactions;
- mempool state;
- witness data;
- and confirmed transactions.

### Stage C: Two-node observer setup

Add an optional second regtest node.

```text
Node A:
wallet and transaction originator

Node B:
independent observer
```

The observer should receive the transaction through the regtest peer connection and inspect it without access to the originating wallet’s private metadata. This setup more clearly demonstrates the distinction between:

- public transaction data;
- and information known only to the wallet.

### Stage D: Automation

Convert validated manual commands into scenario scripts. Each script should:

1. verify that the node is on regtest;
2. create or load the required wallets;
3. obtain sufficient mature regtest funds;
4. construct the test output;
5. mine the funding transaction;
6. decode and save the output;
7. create and broadcast the spend;
8. inspect it before confirmation;
9. mine a block;
10. inspect it after confirmation;
11. and generate an observation record.

## 4. Environment setup tasks

### Task 1: Define supported environment

Document:

- operating system used for initial testing;
- Bitcoin Core version;
- Bash version;
- Python version;
- `jq` version;
- and any descriptor or wallet requirements.

### Task 2: Create isolated regtest data directory

Use a project-specific data directory so the lab does not affect another regtest environment.

Example conceptual location:

```text
./.regtest-data/
```

The actual path should be configurable.

### Task 3: Create wallets

Create separate wallets for:

- mining;
- sender;
- receiver;
- and optional observer operations.

Wallet names should be deterministic within the lab.

### Task 4: Generate mature funds

Mine enough blocks to produce spendable coinbase outputs. The setup script should save:

- miner address;
- generated block hashes;
- wallet balance;
- and current block height.

### Task 5: Record environment metadata

Write an environment file such as:

```json
{
  "network": "regtest",
  "bitcoin_core_version": "record-at-runtime",
  "platform": "record-at-runtime",
  "created_at": "record-at-runtime"
}
```

## 5. Common observation workflow

Every scenario should use the same observation procedure.

### Observation A: Funding output

Save:

- raw funding transaction;
- decoded funding transaction;
- output index;
- address type;
- `scriptPubKey`;
- amount;
- confirmation block;
- and expected initial exposure state.

### Observation B: UTXO while unspent

Record:

- whether the full public key is visible;
- whether a key hash is visible;
- whether a script hash is visible;
- whether a Taproot output key is visible;
- and whether any external key evidence is being supplied.

### Observation C: Pending spend

Broadcast the spend without immediately generating a block.

Save:

- raw pending transaction;
- decoded pending transaction;
- mempool entry;
- `scriptSig`;
- witness;
- public keys;
- scripts;
- internal key or control block, where applicable;
- and updated exposure state.

### Observation D: Confirmed spend

Generate a block and save:

- block hash;
- confirmed transaction;
- confirmation count;
- and whether confirmation changed the public visibility.

### Observation E: Related UTXOs

Where key or script reuse is being tested, identify other UTXOs using the same:

- public key;
- key hash;
- redeem script;
- witness script;
- or script leaf.

Update their expected classifications.

## 6. Scenario implementation plan

## 6.1 Scenario P2PKH

### Goal

Show that a fresh P2PKH output contains a public-key hash and reveals the complete public key through `scriptSig` when spent.

### Steps

- [ ] Create a fresh legacy address.
- [ ] Fund the address.
- [ ] Mine the funding transaction.
- [ ] Decode the funding output.
- [ ] Verify that the output contains a key hash, not the complete key.
- [ ] Create a spending transaction.
- [ ] Broadcast without mining.
- [ ] Decode `scriptSig`.
- [ ] identify the signature and public key.
- [ ] Generate a block.
- [ ] Compare pre-broadcast, pending, and confirmed observations.

### Expected transition

```text
HIDDEN_UNTIL_SPEND
        ↓ broadcast
SHORT_EXPOSURE_ACTIVE
        ↓ confirmation
Output spent; key remains publicly known
```

### Acceptance criteria

- [ ] Funding output saved.
- [ ] Pending spend saved.
- [ ] Public-key location identified.
- [ ] Exposure transition documented.
- [ ] Observation JSON produced.

## 6.2 Scenario P2WPKH

### Goal

Show that a fresh P2WPKH output reveals a public-key hash and that the witness later reveals the signature and complete public key.

### Steps

- [ ] Create a fresh native SegWit v0 address.
- [ ] Fund the address.
- [ ] Mine the funding transaction.
- [ ] Decode the version-0 witness program.
- [ ] Confirm that the complete key is not in the output.
- [ ] Create and broadcast a spend.
- [ ] Inspect the unconfirmed witness.
- [ ] Identify the signature and public key.
- [ ] Confirm the transaction.
- [ ] Record the exposure transition.

### Expected transaction structure

Before spending:

```text
scriptPubKey:
0 <20-byte-key-hash>
```

During spending:

```text
witness:
<signature>
<public-key>
```

BIP141 specifies this two-item witness structure for P2WPKH.

### Acceptance criteria

- [ ] Witness program decoded.
- [ ] Public key absent before spending.
- [ ] Public key identified in pending witness.
- [ ] Result mapped to classifier state.
- [ ] Observation JSON produced.

## 6.3 Scenario key reuse

### Goal

Show how revealing a public key through one spend changes the classification of another unspent output controlled by the same key.

### Steps

- [ ] Generate one P2WPKH or P2PKH address.
- [ ] Create two separate UTXOs to that same address.
- [ ] Verify that both initially contain the same public-key hash.
- [ ] Spend only one UTXO.
- [ ] Broadcast without confirmation.
- [ ] Extract the public key from the spend.
- [ ] Hash the public key and compare it with the second output.
- [ ] Reclassify the second output.
- [ ] Mine the transaction.

### Expected transition for second output

```text
HIDDEN_UNTIL_SPEND
        ↓ key revealed by another UTXO
EXPOSED_BY_REUSE
```

### Acceptance criteria

- [ ] Two distinct UTXOs created.
- [ ] Same key hash demonstrated.
- [ ] One UTXO remains unspent.
- [ ] Revealed key linked to remaining UTXO.
- [ ] Classifier result updated.

## 6.4 Scenario P2WSH

### Goal

Show that the output contains only a script hash and that spending reveals the witness script and its public keys.

### Proposed initial script

Use a simple script that is easy to inspect, such as:

- 1-of-2 multisignature;
- 2-of-2 multisignature;
- or a one-key script with an additional condition.

The first version should prioritize clarity over complexity.

### Steps

- [ ] Create the witness script.
- [ ] Create or import the corresponding P2WSH descriptor/output.
- [ ] Fund the output.
- [ ] Decode the 32-byte witness program.
- [ ] Confirm that the witness script is not visible.
- [ ] Create and broadcast a valid spend.
- [ ] Extract the final witness item.
- [ ] Verify that it is the witness script.
- [ ] Identify public keys and policy in the script.
- [ ] Mine the spend.
- [ ] Record exposure transition.

BIP141 defines P2WSH as a 32-byte witness program whose witness ends with the serialized witness script; the script’s SHA-256 must match the witness program.

### Expected transition

```text
SCRIPT_DEPENDENT_HIDDEN
        ↓ script revealed by broadcast
SHORT_EXPOSURE_ACTIVE or script-specific exposed state
```

### Acceptance criteria

- [ ] Script commitment saved.
- [ ] Hidden script verified before spending.
- [ ] Script extracted from pending witness.
- [ ] Embedded keys identified.
- [ ] Observation JSON produced.

## 6.5 Scenario P2TR key path

### Goal

Show that the x-only output key is visible at funding time and that key-path spending normally reveals only a Schnorr signature.

### Steps

- [ ] Create a native Taproot address.
- [ ] Fund the address.
- [ ] Decode the version-1 witness program.
- [ ] Record the 32-byte x-only output key.
- [ ] Classify the UTXO before spending.
- [ ] Create and broadcast a key-path spend.
- [ ] Inspect the witness.
- [ ] Confirm that no new public key is needed in the witness.
- [ ] Mine the transaction.
- [ ] Record that exposure existed from creation.

### Expected classification

Before spending:

```text
EXPOSED_AT_CREATION
```

During spending:

```text
EXPOSED_AT_CREATION
```

with a pending-spend modifier if desired.

### Acceptance criteria

- [ ] Output key extracted.
- [ ] Initial long-exposure classification recorded.
- [ ] Schnorr signature identified.
- [ ] No false “hidden until spend” classification.
- [ ] Observation JSON produced.

## 6.6 Scenario P2TR script path

### Goal

Show the difference between output-key exposure and script-policy disclosure.

### Proposed approach

Create a Taproot output with at least two script leaves. Example conceptual leaves:

```text
Leaf A:
<key-A> OP_CHECKSIG

Leaf B:
<relative-lock> OP_CHECKSEQUENCEVERIFY OP_DROP
<key-B> OP_CHECKSIG
```

### Steps

- [ ] Create internal key.
- [ ] Create at least two Tapscript leaves.
- [ ] Construct or import the Taproot descriptor.
- [ ] Fund the P2TR output.
- [ ] Confirm that only the output key is visible.
- [ ] Spend one script leaf.
- [ ] Inspect witness stack.
- [ ] Extract selected script.
- [ ] Extract control block.
- [ ] Identify internal key and Merkle sibling hashes.
- [ ] Confirm unused leaf remains undisclosed.
- [ ] Record newly exposed leaf keys.

### Expected findings

```text
Output-key exposure:
active from creation

Script-policy disclosure:
occurs only for selected leaf at spend time
```

### Acceptance criteria

- [ ] Output key recorded before spend.
- [ ] Selected leaf extracted after broadcast.
- [ ] Control block decoded.
- [ ] Unused leaf not disclosed.
- [ ] Observation mapped to both base and additional exposure states.

## 6.7 Scenario broadcast without confirmation

### Goal

Demonstrate that public-key exposure begins at disclosure, not confirmation.

### Steps

- [ ] Create a fresh P2WPKH output.
- [ ] Create and broadcast its spend.
- [ ] Verify that the transaction remains unconfirmed.
- [ ] Inspect the witness and extract the public key.
- [ ] Record the exposure state.
- [ ] Optionally replace, abandon, or leave the transaction unconfirmed.
- [ ] Explain why the key should remain marked as disclosed.

### Acceptance criteria

- [ ] Public key observed while confirmation count is zero.
- [ ] Exposure record created before mining.
- [ ] Documentation does not imply that dropped transactions become secret again.

## 7. Observation schema

Each scenario should output a record similar to:

```json
{
  "scenario_id": "p2wpkh-fresh-spend",
  "environment": {
    "network": "regtest",
    "bitcoin_core_version": "record-at-runtime",
    "node_count": 1
  },
  "funding_output": {
    "txid": "example",
    "vout": 0,
    "output_type": "P2WPKH",
    "script_pub_key": "0014...",
    "complete_public_key_visible": false,
    "initial_exposure_state": "HIDDEN_UNTIL_SPEND"
  },
  "pending_spend": {
    "txid": "example",
    "confirmations": 0,
    "public_key_visible": true,
    "public_key_location": "vin[0].txinwitness[1]",
    "script_revealed": false,
    "exposure_state": "SHORT_EXPOSURE_ACTIVE"
  },
  "confirmed_spend": {
    "confirmations": 1,
    "visibility_changed": false
  },
  "classifier_expected": {
    "primary_state": "SHORT_EXPOSURE_ACTIVE",
    "modifiers": [
      "PENDING_SPEND",
      "CLASSICAL_EC_AUTHORIZATION"
    ]
  },
  "notes": []
}
```
