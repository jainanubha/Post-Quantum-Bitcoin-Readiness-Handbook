# Introduction

Bitcoin is a monetary system built on open-source software, economic incentives, and cryptographic assumptions. Its security does not come from a central operator, a trusted custodian, or any permissioned authority. Instead, Bitcoin is decentralized using public verification, proof-of-work consensus, digital signatures, hash functions, input and output scripts, and a globally replicated ledger to allow users to hold and transfer value without relying on trusted intermediaries.

That design is powerful, but it also means Bitcoin must be safe with a view for a long time horizon. Its design including the cryptographic assumptions that are safe today may need to be reassessed as computing capabilities change with time. Post-quantum cryptography is one such area. A sufficiently capable quantum computer could threaten some public-key cryptographic systems that are widely used today, including the elliptic-curve signature schemes used in Bitcoin. This does not mean Bitcoin is immediately broken, and it does not mean there is a simple or urgent consensus change ready to deploy. It means Bitcoin developers, wallet builders, educators, and researchers need a careful way to understand the risk surface, separate realistic concerns from hype, and evaluate threats and possible migration paths.

This handbook is intended to support that process for the stakeholders.

## What this handbook is about

The **Post-Quantum Bitcoin Readiness Handbook** is an open-source educational and research-readiness project. It is designed to help readers understand what post-quantum readiness means specifically for Bitcoin.

The focus is not general quantum computing, nor is it a broad introduction to all of post-quantum cryptography. The focus is Bitcoin-specific:

* What cryptographic assumptions does Bitcoin rely on today?
* Where are public keys exposed in Bitcoin transactions?
* How do different Bitcoin output types differ in their quantum-risk profile?
* What is the difference between long-exposure and short-exposure quantum attacks?
* How do NIST-standardized post-quantum cryptographic constructions relate to Bitcoin’s needs?
* Why are some post-quantum signatures difficult to adopt in Bitcoin?
* What do current Bitcoin proposals such as BIP360 and BIP361 attempt to address?
* What remains open, controversial or not yet ready for deployment?
* How should wallet developers, exchanges, custodians, educators, and users think about readiness?

The handbook will combine explanatory chapters, curated reading lists, proposal summaries, diagrams, glossary entries, small developer labs, and open research questions. The goal is to make a scattered and fast-evolving space easier to navigate in a structured way.

## Why Bitcoin needs a specific post-quantum readiness discussion

Post-quantum migration is not the same problem in every system.

Many internet systems can migrate cryptographic protocols through software updates, protocol negotiation, or centralized infrastructure decisions. Bitcoin is different. It is decentralized, conservative by choice and design, and extremely sensitive to any consensus changes. Any serious post-quantum migration discussion must consider not only cryptographic security, but also:

* consensus compatibility
* soft-fork design
* transaction weight
* block space usage
* fee impact
* validation cost
* wallet support
* hardware wallet support
* UTXO migration
* old and dormant coins
* user education
* miner and node policy
* exchange and custodian readiness
* Bitcoin’s social consensus

This means that “choose a post-quantum signature scheme” is only one part of the problem. A signature scheme may be secure in a cryptographic standard, but still be difficult to use in Bitcoin if signatures are too large, validation is too expensive, wallet backup becomes fragile or migration creates unacceptable pressure on users and the fee market.

Bitcoin post-quantum readiness is therefore both a cryptographic problem and a systems problem.

## The public-key exposure problem

A core issue in Bitcoin’s quantum-risk model is public-key exposure.

Bitcoin signatures prove that a spender controls the private key corresponding to a public key. In many Bitcoin output types, the public key is not fully visible until the coin is spent. In other cases, public keys are exposed earlier and for much longer. This difference matters because a quantum attack against elliptic-curve signatures requires access to the public key.

A useful first distinction is:

* **Long-exposure risk:** the public key is already visible for a long time before the coin is spent or moved.
* **Short-exposure risk:** the public key becomes visible only when the user broadcasts a spending transaction, creating a shorter attack window in the mempool before confirmation.

This distinction is central to understanding current Bitcoin post-quantum discussions. Some proposals aim to reduce long-term public-key exposure. Others focus on future post-quantum signatures or migration mechanisms. These are related but not identical problems.

For example, an output design that avoids long-term public-key exposure may still not solve the problem of a public key revealed in the mempool during spending. Conversely, a future post-quantum signature scheme may help with spend authorization but still needs a practical way to integrate with Bitcoin’s consensus rules, wallet infrastructure and fee market.

## Current standards and Bitcoin-specific proposals

In 2024, NIST finalized the first set of post-quantum cryptography standards, including ML-KEM for key encapsulation and ML-DSA and SLH-DSA for digital signatures. These standards are important starting points for studying post-quantum cryptography, but Bitcoin cannot adopt them automatically. Bitcoin has its own constraints around consensus validation, transaction size, witness data, fee pressure, wallet migration and long-term compatibility.

At the Bitcoin proposal level, two recent draft BIPs are especially relevant to this handbook for now:

* **BIP360: Pay-to-Merkle-Root (P2MR):** a proposed output type similar to Taproot script trees, but without the quantum vulnerable Taproot key-path spend. It is best understood as a proposal related to reducing long-exposure public-key risk, not as a complete post-quantum signature solution.
* **BIP361: Post Quantum Migration and Legacy Signature Sunset:** a proposed migration framework that discusses how Bitcoin could move away from quantum vulnerable legacy signatures after a post-quantum output type exists. It raises technical, economic, and social questions about migration deadlines, rescue mechanisms and old coins.

These proposals are not treated in this handbook as final answers. They help reveal the structure of the problem: output types, signature schemes, migration incentives, public-key exposure and Bitcoin’s social contract all interact.

## What this handbook is not

This handbook does not claim that Bitcoin will be broken by a specific date.

It does not claim that any currently discussed proposal is ready for activation.

It does not endorse one post-quantum signature scheme as the final choice for Bitcoin.

It does not provide financial advice, investment advice or emergency migration advice.

It does not attempt to implement any consensus critical cryptography.

Instead, this handbook aims to provide a neutral and technically grounded learning path. Where a topic is settled, the handbook will say so. Where a topic is still under discussion, the handbook will mark it as such. Where a proposal is draft or experimental, the handbook will not present it as deployed consensus.


## Who this handbook is for

This handbook is written for:

* Bitcoin developers who want to understand post-quantum migration discussions.
* Wallet developers who want to reason about public-key exposure and future readiness.
* Educators who want to teach the topic with technical depth.
* Researchers interested in Bitcoin cryptography, privacy and long-term protocol security.
* Students and new contributors looking for a structured entry point into advanced Bitcoin topics.
* Reviewers who want a map of proposals, papers, standards and open questions.

The expected reader does not need to be a post-quantum cryptography expert. However, some familiarity with Bitcoin transactions, public and private keys, digital signatures and basic cryptographic terminology will be helpful. The handbook begins with Bitcoin cryptography basics before moving into quantum-risk models and post-quantum proposals.

## How the handbook is organized

The project is organized into several parts.

### 1. Foundations

The first part introduces Bitcoin cryptography, including ECDSA, Schnorr signatures, hashes, commitments, Taproot upgrade and the role of public keys in transaction authorization.

### 2. Quantum-risk model for Bitcoin

The second part explains how quantum algorithms affect Bitcoin-relevant cryptographic primitives. It distinguishes between threats to signatures and threats to hash functions and introduces long-exposure and short-exposure attack models.

### 3. Post-quantum cryptography standards

The third part introduces NIST-standardized post-quantum constructions, especially ML-DSA and SLH-DSA as digital-signature schemes. It also explains why key encapsulation mechanisms such as ML-KEM are important in general cryptography but do not directly solve Bitcoin transaction signing.

### 4. Bitcoin proposal design space

The fourth part studies Bitcoin-specific proposals and discussions, including BIP360, BIP361, post-quantum output types, hash-based signatures, witness-version approaches, Taproot script-path ideas and migration debates.

### 5. Developer labs and simulations

The fifth part will include small educational tools and examples, such as:

* signature-size comparison tables
* transaction-weight and fee estimators
* UTXO exposure classifiers
* regtest examples showing when public keys become visible
* proposal explainer notebooks

### 6. Readiness checklists and open problems

The final part will collect wallet readiness considerations, exchange and custodian checklists, educator notes, unresolved research questions and contribution paths for future Bitcoin developers.

## How to read this handbook

Readers new to the topic should begin with the Bitcoin cryptography basics and the quantum-risk model. These sections provide the vocabulary needed to understand later chapters.

A suggested path is:

1. Read the introduction.
2. Review Bitcoin signatures, hashes, and address types.
3. Study public-key exposure across output types.
4. Learn the difference between long-exposure and short-exposure attacks.
5. Read the NIST PQC standards overview.
6. Compare post-quantum signature schemes through Bitcoin constraints.
7. Study BIP360 and BIP361.
8. Review open questions and proposal tradeoffs.
9. Try the developer labs.
10. Contribute corrections, summaries, or review comments.

Advanced readers may prefer to start from the proposal tracker, academic papers, or developer labs.

## Contribution philosophy

This project is intended to be built in public.

Corrections, review comments, resource suggestions, glossary improvements and proposal-tracker updates are welcome. Since the topic combines Bitcoin protocol design, cryptography, wallet engineering and social consensus, no single contributor is expected to have complete expertise in every area.

Useful contributions include:

* improving a technical explanation
* adding a missing primary source
* clarifying a draft proposal
* correcting terminology
* identifying unsupported claims
* improving diagrams
* adding examples
* reviewing developer labs
* documenting open questions

All contributions should aim for clarity, accuracy, neutrality and usefulness. The goal is to help the Bitcoin community understand the design space more clearly.

## Status of the project

This handbook is in an early stage.

The first milestone is to establish the repository, define the scope, add licensing and contribution guidelines, create the initial reading lists, and draft the first chapters. Later milestones will expand the handbook, add proposal explainers, create developer labs, invite technical review and prepare a public versioned release.

The project will evolve as Bitcoin post-quantum discussions evolve. Readers should treat the handbook as a living resource.
