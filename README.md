# NullCat

NullCat (NC) is a planned independent blockchain and native currency designed from scratch in Rust. Its purpose is to support a decentralized economy in which open-source developers can earn compensation. This repository, `net-cat`, is in the specification phase and covers blockchain infrastructure.

[LegacyBound.dev](https://legacybound.dev) is the planned Git forge for verifiable repository ownership, authorization and rewards. NullCat Wallet is the planned non-custodial, multi-chain wallet. Each is a separate project. Forge Bounties (`make-money`) is unrelated.

- [Whitepaper, draft v0.2.0](WHITEPAPER.md)
- [Architectural decision log](docs/DECISIONS.md)
- [Open questions and dependencies](docs/OPEN_QUESTIONS.md)
- [Contributor rules](AGENTS.md)
- [Proposed license for legal review](LICENSE.draft.md)

## Specifications

| Domain | Specifications |
| --- | --- |
| Economics | [Monetary protocol](docs/specs/MONETARY_PROTOCOL.md), [Rewards](docs/specs/REWARDS.md) |
| Chain | [Consensus](docs/specs/CONSENSUS.md), [Networking](docs/specs/NETWORKING.md), [Ledger](docs/specs/LEDGER.md) |
| Applications | [Repositories](docs/specs/REPOSITORIES.md), [Identity](docs/specs/IDENTITY.md), [Wallet](docs/specs/WALLET.md) |
| Cross-cutting | [Security](docs/specs/SECURITY.md), [Governance](docs/specs/GOVERNANCE.md) |

Established requirements constrain design. Proposed mechanisms require approval. Open questions record choices that remain undecided.

## Licensing

[BSD-3-Clause](LICENSE) is the active license. [NCOSL](LICENSE.draft.md) is a proposed permissive license derived from BSD-3-Clause and pending professional legal review. The project intends to transition only after legal review, explicit adoption and authorization from the relevant rights holders.

Contributor copyright stays with its holders unless separately transferred. Contributions are accepted under BSD-3-Clause. Keep authorship and third-party provenance with each contribution. Before relicensing, obtain and retain explicit written authorization from the relevant rights holders for the final license text and covered contributions, or through a separately reviewed contributor agreement that covers the change. A contribution or sign-off alone does not grant relicensing authorization. If clearance is unavailable, keep the affected material under its existing terms where legally permissible, replace it or omit it from the relicensed release. Existing BSD-3-Clause releases retain their terms.
