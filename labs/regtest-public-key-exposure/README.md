# Regtest Public-Key Exposure Lab

> **Status:** Design and early implementation  
> **Version:** `v0.1-dev`  
> **Network:** Bitcoin Core regtest
> **Purpose:** Demonstrate when Bitcoin public keys and spending scripts become visible before, during and after spending

## Overview

The Regtest Public-Key Exposure Lab is an educational developer lab which demonstrate, using reproducible Bitcoin Core regtest transactions, how public-key exposure differs across common Bitcoin output types. The lab focuses on observable transaction data rather than implementing a quantum attack. It examines:

- what an observer can see when an output is created;
- what remains hidden while the output is unspent;
- what becomes visible when a spending transaction is broadcast;
- what remains permanently visible after confirmation;
- how key and script reuse affect other UTXOs;
- and how these observations relate to long-exposure and short-exposure quantum attacks.

The initial scenarios cover:

- Pay to Public Key Hash (`P2PKH`);
- Pay to Witness Public Key Hash (`P2WPKH`);
- Pay to Witness Script Hash (`P2WSH`);
- Pay to Taproot (`P2TR`) key-path spending;
- and, where feasible, P2TR script-path spending.

The project task list specifically calls for regtest examples comparing P2PKH, P2WPKH, P2TR, and script-based outputs and explaining what an observer learns before and after spending. This lab implements that requirement.

## Learning objectives

After completing the lab, a participant should be able to:

1. Identify the output script of a funded UTXO.
2. Distinguish a public key from a public-key hash.
3. Identify where signatures and public keys appear in transaction inputs.
4. Explain why fresh P2PKH and P2WPKH outputs hide the full public key until spending.
5. Explain why P2TR exposes an x-only output key from creation.
6. Explain why P2WSH hides a witness script until spending.
7. Observe public-key disclosure in an unconfirmed transaction.
8. Explain why confirmation is not required for a key to become exposed.
9. Demonstrate how key or script reuse affects other unspent outputs.
10. Map transaction observations to the UTXO Exposure Classifier.

## What this lab does

The lab creates controlled regtest transactions and records their exposure lifecycle. For each scenario, it inspects:

```text
1. Funding address or output template
2. Funding transaction
3. Created UTXO
4. Unconfirmed spending transaction
5. Confirmed spending transaction
6. Related reused outputs, where applicable
```

The lab records:

- `scriptPubKey`;
- address or output type;
- witness version and program;
- `scriptSig`;
- witness stack;
- public keys;
- script commitments;
- revealed scripts;
- control blocks, where applicable;
- and the corresponding exposure classification.

## Relationship to handbook content

This lab supports the following handbook material:

- [`Bitcoin Cryptography Refresher`](../../handbook/03-bitcoin-cryptography-refresher.md)
- [`Shor’s Algorithm and the Discrete-Log Threat`](../../handbook/04-shors-algorithm-and-discrete-log-threat.md)
- [`Grover’s Algorithm and Bitcoin Hashes`](../../handbook/05-grovers-algorithm-and-bitcoin-hashes.md)
- [`Bitcoin Address Types and Public-Key Exposure`](../../handbook/06-bitcoin-address-types-and-public-key-exposure.md)
- [`Long-Exposure versus Short-Exposure Quantum Attacks`](../../handbook/07-long-exposure-vs-short-exposure-attacks.md)
- [`Taproot Key-Path and Script-Path Considerations`](../../handbook/08-taproot-key-path-and-script-path-considerations.md)
- [`Bitcoin Address-Type Exposure Matrix`](../../tables/address-type-exposure-matrix.md)

It also produces inputs and expected results for:

- [`UTXO Exposure Classifier`](../utxo-exposure-classifier/README.md)
- [`UTXO Exposure Classification Model`](../utxo-exposure-classifier/exposure-model.md)

## Prerequisites

The lab is expected to require:

- Bitcoin Core with `bitcoind` and `bitcoin-cli`;
- a Unix-like shell such as Bash;
- `jq` for inspecting JSON responses;
- Python 3 for optional verification scripts;
- and Git.

## Proposed lab environment

The minimum version may use:

- one Bitcoin Core regtest node;
- one mining wallet;
- one sender wallet;
- and one receiver wallet.

An expanded version may use two nodes:

```text
Node A:
creates and broadcasts transactions

Node B:
acts as an independent observer
```

## Standard scenario lifecycle

Every scenario should follow the same general lifecycle.

### Step 1: Create the output

Create an address or script for the selected output type. Record:

- address;
- output type;
- descriptor, if applicable;
- relevant public-key hash, script hash, or output key;
- and expected initial exposure category.

### Step 2: Fund the output

Send regtest bitcoin to the output and mine a block. Record:

- funding transaction ID;
- output index;
- amount;
- decoded `scriptPubKey`;
- and confirmation status.

### Step 3: Inspect the unspent output

Before spending, inspect the UTXO. Answer:

- Is a complete public key visible?
- Is only a public-key hash visible?
- Is only a script commitment visible?
- Is an x-only Taproot output key visible?
- What can an observer infer at this stage?

### Step 4: Create and broadcast the spend

Create a transaction spending the UTXO. Broadcast it without immediately mining a block. Record:

- spending transaction ID;
- mempool status;
- `scriptSig`;
- witness stack;
- revealed public keys;
- revealed scripts;
- control block, where applicable;
- and updated exposure category.

### Step 5: Inspect from the observer’s perspective

Inspect the raw transaction as a public observer would. Do not rely only on wallet-specific metadata. Answer:

- Which values are visible in the serialized transaction?
- Which values require wallet knowledge?
- Has a previously hidden public key become visible?
- Has a script been revealed?
- Would any other reused output now inherit the exposure?

### Step 6: Confirm the transaction

Mine a block and confirm the spend. Record:

- block hash;
- confirmation count;
- final decoded transaction;
- and whether the visibility changed at confirmation.

The expected observation is that key or script disclosure normally occurs at broadcast, not only at confirmation.

## Expected outputs

The lab should eventually produce:

- decoded funding transactions;
- decoded spending transactions;
- pre-spend and post-broadcast observations;
- exposure-classifier input records;
- expected classifier outputs;
- and concise explanations of each visibility transition.

## Contributing

Contributions may include:

- additional output-type scenarios;
- command corrections;
- cross-platform setup instructions;
- expected-output fixtures;
- regtest automation;
- multi-node observer configurations;
- and technical review.

Before contributing, read:

- [`CONTRIBUTING.md`](../../CONTRIBUTING.md)
- [`exposure-model.md`](../utxo-exposure-classifier/exposure-model.md)
- [`address-type-exposure-matrix.md`](../../tables/address-type-exposure-matrix.md)
