# Contributing to Post-Quantum Bitcoin Readiness Handbook

Thank you for your interest in contributing to the **Post-Quantum Bitcoin Readiness Handbook**.

This project is an open-source handbook and developer labs for helping Bitcoin developers, educators, wallet builders, and researchers understand Bitcoin’s post-quantum risk surface and migration design space.

The goal is to create a technically correct and accessible resource covering Bitcoin cryptography, public-key exposure, long-exposure and short-exposure attacks, NIST post-quantum cryptography standards, post-quantum signature trade-offs, Bitcoin migration proposals, implementation resources, and open research questions.

This project does **not** endorse a specific Bitcoin consensus change. Contributions should improve clarity, accuracy and usefulness for the Bitcoin community.

---

## Table of contents

- [Project goals](#project-goals)
- [What this project is not](#what-this-project-is-not)
- [Ways to contribute](#ways-to-contribute)
- [Contribution categories](#contribution-categories)
- [Resource submission format](#resource-submission-format)
- [Writing guidelines](#writing-guidelines)
- [Technical accuracy policy](#technical-accuracy-policy)
- [Copyright and source policy](#copyright-and-source-policy)
- [Proposal tracker policy](#proposal-tracker-policy)
- [Developer lab contribution policy](#developer-lab-contribution-policy)
- [Issue and pull request process](#issue-and-pull-request-process)
- [Review expectations](#review-expectations)
- [Suggested first contributions](#suggested-first-contributions)
- [Code of conduct](#code-of-conduct)
- [License](#license)

---

## Project goals

The project aims to build a public, versioned, community-reviewed resource for studying post-quantum readiness in Bitcoin.

The main goals are:

1. Explain Bitcoin’s current cryptographic assumptions.
2. Explain how quantum computing may affect ECDSA, Schnorr signatures, and hash functions.
3. Classify Bitcoin output types by public-key exposure and quantum-risk model.
4. Distinguish long-exposure attacks from short-exposure attacks.
5. Summarize NIST post-quantum cryptography standards relevant to Bitcoin.
6. Compare post-quantum signature schemes and their Bitcoin-specific constraints.
7. Track Bitcoin post-quantum proposals and discussions.
8. Build educational examples, diagrams, tables and developer labs.
9. Create a useful learning path for Bitcoin developers and educators.
10. Identify open questions that require further research and review.

---

## What this project is not

This project is **not**:

- A proposal to activate an immediate Bitcoin consensus change.
- An endorsement of any specific post-quantum signature scheme.
- A source of financial, legal, or investment advice.
- A replacement for formal BIPs, peer-reviewed cryptographic analysis, or Bitcoin Core review.
- A repository for mirroring full third-party papers, books, standards, or articles.

Contributors should avoid language such as:

- “Bitcoin is already broken.”
- “This proposal solves quantum risk completely.”
- “This scheme is definitely the final answer for Bitcoin.”
- “Quantum computers will certainly break Bitcoin by year X.”

Instead, prefer careful language such as:

- “This proposal aims to address…”
- “This resource discusses…”
- “This remains an open question…”
- “This depends on the assumed attacker model…”
- “This is a draft proposal and should not be treated as consensus.”

---

## Ways to contribute

You can contribute in several ways:

1. Suggest a new resource.
2. Improve an existing reading-list entry.
3. Correct a technical inaccuracy.
4. Improve a chapter draft.
5. Add a glossary term.
6. Add a diagram or visual explanation.
7. Add a proposal-tracker entry.
8. Add a developer lab or simulation.
9. Review an explanation for clarity.
10. Open an issue describing an open research question.
11. Improve repository structure, navigation, or documentation.
12. Help classify resources by difficulty, status, and Bitcoin relevance.

---

## Contribution categories

### 1. Reading-list contributions

Reading-list contributions include papers, standards, BIPs, Delving Bitcoin threads, bitcoin-dev discussions, implementation repositories, educational articles, and other useful resources.

A good reading-list contribution should explain:

- What the resource is.
- Why it matters for post-quantum Bitcoin readiness.
- Whether it is beginner, intermediate, or advanced.
- Whether it is a standard, draft proposal, research paper, implementation, or opinion piece.
- What a reader should focus on.

### 2. Handbook content

Handbook content should be original explanatory writing. It should help readers understand a concept, proposal, risk model, or tradeoff.

Possible chapter topics include:

- Why post-quantum readiness matters for Bitcoin.
- Bitcoin cryptography refresher: ECDSA, Schnorr, hashes, and commitments.
- Shor’s algorithm and the discrete-log threat.
- Grover’s algorithm and what it does.
- Bitcoin address types and public-key exposure.
- Long-exposure versus short-exposure quantum attacks.
- Taproot, key-path spends, and script-path considerations.
- NIST post-quantum standards.
- Post-quantum signature schemes and Bitcoin constraints.
- Transaction weight, fees, and block-space implications.
- BIP360: Pay-to-Merkle-Root.
- BIP361: migration and legacy-signature sunset.
- Wallet, exchange, and custodian readiness checklists.
- Open problems and contribution paths.

### 3. Developer labs and simulations

Developer labs should be educational and reproducible. They should help readers understand a concept through small examples, scripts, notebooks, or tables.

Possible labs include:

- Signature-size comparison table.
- Transaction-weight and fee estimator.
- UTXO exposure classifier.
- Regtest examples showing when public keys become visible.
- BIP360 explainer notebook.
- BIP361 explainer notebook.
- “What would a PQ signature do to transaction size?” notebook.

Developer labs are not intended to be consensus-ready Bitcoin implementations.

### 4. Proposal tracker contributions

Proposal tracker entries should summarize Bitcoin post-quantum proposals and discussions.

A proposal tracker entry should include:

- Proposal or discussion name.
- Link to primary source.
- Author or contributor names.
- Status.
- Main idea.
- What problem it addresses.
- What it does not solve.
- Open questions.
- Related discussions.

### 5. Glossary contributions

Glossary entries should be short, clear, and technically careful.

Examples of useful glossary terms:

- Post-quantum cryptography
- Shor’s algorithm
- Grover’s algorithm
- secp256k1
- ECDSA
- Schnorr signature
- Public-key exposure
- Long-exposure attack
- Short-exposure attack
- ML-DSA
- SLH-DSA
- BIP360
- BIP361
- Taproot key-path spend
- Taproot script-path spend
- Quantum-safe migration

---

## Resource submission format

When suggesting a new resource, please use the following format:

### [Resource title](https://example.com)

**Type:** Paper / BIP / standard / discussion / implementation / article / book / video  
**Source:** Author, organization, project, or forum  
**Year / status:** Year, draft, final, discussion, preprint, etc.  
**Difficulty:** Beginner / Intermediate / Advanced  
**Bitcoin relevance:** Explain why this matters for Bitcoin specifically.  
**Key takeaway:** Summarize the main idea in your own words.  
**Suggested reading order:** Mention what should be read before or after this.  
**Tags:** `pqc`, `bitcoin`, `signatures`, `taproot`, `migration`, etc.  
**Last checked:** YYYY-MM-DD

---

## Writing guidelines

Please write in a clear, neutral, and technically careful style.

Good contributions should:

- Use simple language where possible.
- Define technical terms before using them heavily.
- Distinguish finalized standards from draft proposals.
- Distinguish deployed Bitcoin behavior from future design ideas.
- Avoid hype and unsupported predictions.
- Mention uncertainty where the design space is unsettled.
- Link to primary sources for technical claims.

Avoid:

- Copying large passages from external sources.
- Treating draft BIPs as accepted consensus.
- Presenting one signature scheme as the obvious final solution.
- Making claims about exact quantum timelines without strong qualification.
- Overstating what any proposal solves.

---

## Technical accuracy policy

Post-quantum Bitcoin readiness is an emerging topic. Accuracy is more important than speed.

When making technical claims, please:

- Link to a primary source where possible.
- Clearly state whether something is: deployed/standardized/draft/experimental/under discussion/speculative/an open question.
- Clearly distinguish between: output types, signature schemes, migration policies, wallet practices and consensus changes.

For example:

- BIP360 / P2MR should be described as an output-type proposal, not as a complete post-quantum signature scheme.
- BIP361 should be described as a migration and legacy-signature sunset proposal, not as a signature algorithm.
- ML-KEM should not be described as a Bitcoin transaction signature scheme.
- ML-DSA and SLH-DSA are post-quantum digital signature standards, but Bitcoin has not selected either as a consensus mechanism.

If you are unsure about a claim, open an issue instead of making the change directly.

---

## Copyright and source policy

This repository is a curated educational guide. It links to external papers, standards, BIPs, discussions, articles and software projects.

Do not add full third-party papers, standards, book chapters, blog posts, or articles unless their license explicitly allows redistribution.

Allowed:

- Linking to the original source.
- Writing your own summary.
- Writing your own explanation of Bitcoin relevance.
- Quoting very short excerpts when necessary and properly attributed.
- Adding metadata such as author, year, status, difficulty, and tags.

Not allowed:

- Uploading full copyrighted PDFs without permission.
- Copying full abstracts, large tables, diagrams, or article text.
- Reusing images or diagrams without checking the license.
- Presenting third-party material as original project content.

Third-party resources remain under their original licenses and copyrights.

---

## Proposal tracker policy

The proposal tracker is intended to help readers understand the Bitcoin post-quantum design space.

Inclusion in the tracker does not mean endorsement.

When adding a proposal, please include:

## Proposal name

**Primary link:**  
**Author(s):**  
**Status:** Draft / discussion / research / implementation / obsolete / unknown  
**Type:** Output type / signature scheme / migration policy / opcode / wallet design / recovery mechanism / other  
**Main idea:**  
**What it addresses:**  
**What it does not address:**  
**Bitcoin relevance:**  
**Open questions:**  
**Related resources:**

Proposal summaries should be descriptive.

---

## Issue and pull request process

Opening an issue

Please open an issue if you want to:

- Suggest a new resource.
- Report a technical correction.
- Propose a new chapter.
- Suggest a glossary term.
- Discuss terminology.
- Ask for technical review.
- Suggest a developer lab.
- Track an open question.

Use a clear title, such as:
[Resource] Add NIST FIPS 204 ML-DSA
[Correction] Clarify difference between BIP360 and PQ signatures
[Glossary] Add short-exposure attack
[Chapter] Improve public-key exposure explanation
[Lab] Add transaction-weight estimator

Opening a pull request

Before opening a pull request:

- Check whether a related issue already exists.
- Keep changes focused.
- Use clear commit messages.
- Add sources for technical claims.
- Avoid unrelated formatting changes.
- Preview Markdown files before submitting.
- Explain what changed and why.

A good pull request description includes:

## Summary

Briefly explain the change.

## Type of change

- [ ] New resource
- [ ] Technical correction
- [ ] Chapter content
- [ ] Glossary entry
- [ ] Proposal tracker update
- [ ] Developer lab
- [ ] Documentation / repository maintenance

## Sources

List the main sources used.

## Checklist

- [ ] I have used primary sources where possible.
- [ ] I have not copied large third-party content.
- [ ] I have distinguished facts from interpretation.
- [ ] I have marked draft proposals as draft.
- [ ] I have checked links.
- [ ] I have followed the project’s license and source policy.

---

## Review expectations

Contributions may be reviewed for:

- Technical accuracy.
- Clarity.
- Neutrality.
- Source quality.
- Copyright safety.
- Bitcoin relevance.
- Usefulness to developers and educators.

Some contributions may require review from people familiar with Bitcoin Script, wallets, cryptography, post-quantum signatures, or Bitcoin protocol design.

Review comments are part of the process. Please treat them as collaborative improvement, not personal criticism.

---

## Suggested first contributions

Good first contributions include:

- Add a missing primary source to a reading list.
- Improve the summary of an existing resource.
- Add difficulty level or tags to a resource.
- Add a glossary definition.
- Fix broken links.
- Add a short “why this matters for Bitcoin” note.
- Open an issue for an unclear technical question.
- Improve README navigation.
- Add a diagram idea as an issue.
- Add a proposal tracker entry.

---

## Code of conduct

This project is intended to support serious, respectful technical learning.

Please:

- Be respectful.
- Assume good faith.
- Critique ideas, not people.
- Avoid hype, fear-mongering, or promotional claims.
- Be open to correction.
- Help make the material clearer for future learners.

---

## License

Unless otherwise stated:

- Handbook text, reading lists, diagrams, glossary entries, notes, and educational material are licensed under the license specified for project content.
- Code, scripts, notebooks, and software examples are licensed under the repository’s software license.

Third-party papers, articles, BIPs, specifications, discussions, and linked resources remain under their original licenses and copyrights.

Please see:

LICENSE
LICENSE-CONTENT.md

for the current licensing details.

---

## Contact and maintainership

This is a new self-initiated FOSS education and research project.

The founding maintainer is responsible for initial scope, repository structure, and review coordination. Over time, the project may invite feedback from mentors, Bitcoin developers, wallet developers, educators, and post-quantum cryptography researchers.

If you are interested in reviewing the technical direction of the project, please open an issue or start a discussion.
