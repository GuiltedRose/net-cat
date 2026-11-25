# NullCat technical whitepaper

**Draft v0.2.0**

## Purpose and scope

NullCat (NC) is an independent cryptocurrency and blockchain planned from scratch in Rust. NC is the chain's native currency. NullCat aims to support a decentralized economy in which open-source developers can receive compensation while retaining their software freedoms. LegacyBound.dev will use the chain for repository ownership, authorization and rewards. NullCat Wallet will provide non-custodial access. Blockchain, forge and wallet development belong in separate repositories.

The requirements below constrain the design. Proposed mechanisms require approval, and the [open-questions register](docs/OPEN_QUESTIONS.md) tracks unresolved choices. This draft defines no production parameters beyond the stated design targets.

## Monetary accounting

The proposed maximum outstanding supply is **100 trillion NC**. Genesis will mint a few million NC, with exact quantity and allocation undecided. All network fees are burned. A valid burn reduces outstanding supply and creates room for subsequent issuance. Cumulative lifetime issuance can therefore exceed the cap while outstanding supply remains bounded at every committed transition. NC can be held, transferred, bought and sold through supporting exchanges independently of LegacyBound. Network participation requires no developer account.

Rewards are minted under deterministic protocol rules. The protocol has no prefunded reward pool, developer tax, privileged developer wallets or automatic founder allocations. Developers participate under the same monetary rules as other users. Staking entry irreversibly burns a dynamically determined share of eligible holdings based on individual holdings and network monetary conditions. The design must preserve earning access despite wealth concentration. It must consider whether splitting holdings across identities defeats the burn policy and whether entry burns and consensus collateral serve distinct roles.

A proposed accounting model specifies all value changes in integer atomic units and reconciles outstanding supply against cumulative valid issuance and burns. Balance locks and transfers leave outstanding supply unchanged. The protocol needs explicit ordering and failure rules when competing mints seek the same headroom. Atomic precision, issuance formulas, staking calculations and genesis allocation remain open. See the [monetary specification](docs/specs/MONETARY_PROTOCOL.md).

### Optional secondary asset

An account-bound asset may be earned through eligible participation. It would have no independent exchange listing. Its variable conversion ratio against NC is conceptually similar to the exchange-rate behavior of some liquid staking assets against SOL. Redeemable value may increase over time. Redemption destroys the redeemed units and delivers NC.

Redemption liabilities must fit within the monetary cap. A proposed mechanism reserves unminted headroom for guaranteed liabilities and debits the reservation on settlement. Minted balances held for payment already count toward outstanding supply. Any guarantee would need a rule for liability growth, insufficient headroom and competing claims before it could be offered. Adoption, name, issuance, conversion, accrual and redemption terms remain open. Cryptographic CTF challenges are outside the current scope.

## Consensus, ledger and network

P2P networking is fundamental. Independent operation must be possible without a mandatory central gateway or authority. Consumer hardware and ordinary residential links should support meaningful participation. The ledger and network should remain lightweight, avoiding Solana-scale hardware and bandwidth demands. Compact, deterministic binary serialization is required.

Candidate consensus protocols must be evaluated for safety, finality, validator selection, resource demand, adversarial participation and recovery from partitions. Consensus, collateral, penalties, block size and timing remain undecided. Proposed ledger designs include authenticated snapshots for state sync, pruning, light clients and independently operated archival nodes. Authenticated synchronization depends on a verifiable state commitment and a way to establish its consensus status. Resource budgets must include initial sync and catch-up as well as steady-state participation. See [consensus](docs/specs/CONSENSUS.md), [ledger](docs/specs/LEDGER.md) and [networking](docs/specs/NETWORKING.md).

## Repository registry and replication

LegacyBound will register public and private repositories on-chain. Signed records establish repository ownership and permission changes. Git contents stay off-chain and are distributed over P2P infrastructure that can operate independently of blockchain validation. Public contents require no retrieval authorization. Private contents are encrypted, and each supplier must verify explicit cryptographic authorization before transferring private data. Unauthorized nodes must receive no private repository contents.

Revocation prevents future authorized synchronization and cannot erase downloaded copies. Privacy-preserving metadata and permission representations remain research topics. Candidate approaches include commitments and proofs bound to the requesting party and the current authorized state. Authorization depends on how suppliers establish state freshness and finality during delays or partitions. Replication incentives, routing, key rotation and privacy leakage through network activity remain open. See the [repository specification](docs/specs/REPOSITORIES.md).

## Developer identity and earnings

Developer accounts exist independently of individual wallet addresses and may have no linked wallets. Linking requires cryptographic proof of wallet ownership. Ordinary removal requires approval from both the account and the associated wallet. Wallets have no unilateral control over developer accounts. An independent recovery mechanism must replace compromised wallets while preserving repository ownership, contribution history and account-controlled funds. Financial keys and developer authorization keys should be isolated where practical.

Accounts without eligible linked wallets do not accrue monetary participation rewards. Pending earnings remain under on-chain account control until scheduled payout or redemption. Wallet replacement does not transfer repository ownership. Proposals include distinct account and wallet authorities and settlement records to prevent duplicate payouts. Contribution eligibility, attribution, valuation, payout cadence and recovery procedures remain open. Deterministic settlement depends on a verifiable rule for recognizing off-chain contribution evidence. See [identity](docs/specs/IDENTITY.md) and [rewards](docs/specs/REWARDS.md).

## Wallets and signing

NullCat Wallet is a separate non-custodial project. The browser extension comes first, with possible native apps later. It will support NC and interact with BTC, ETH and SOL through their respective native networks without requiring bridges. NC will have a dedicated Ledger device application. Supported external currencies may use existing Ledger applications.

Transaction serialization must accommodate embedded hardware limits and clear signing. Candidate formats should give hardware signers enough context to show the operation, network, recipient, amount and fees as applicable. Private keys and recovery secrets must never reach network nodes. Signing formats and key conventions remain undecided. See [wallet integration](docs/specs/WALLET.md).

## Licensing, governance and adoption

Fully open-source infrastructure is a project objective. GitHub is temporary development infrastructure. NullCat intends to host its source code through LegacyBound once the forge can support it. Governance must grant no automatic privileges to founders, developers or investors. Governance participation, protocol upgrades and emergency recovery procedures remain open.

[BSD-3-Clause](LICENSE) is the active license. The project intends to transition to the proposed [NullCat Open Source License](LICENSE.draft.md), a permissive draft derived from BSD-3-Clause, subject to professional legal review, explicit adoption and documented rights-holder authorization. Contributor rights and the handling of existing releases are described in the [README](README.md#licensing). The proposal preserves proprietary derivative rights and keeps software freedoms independent of network use, cryptocurrency ownership, royalties, wallet links and hosting providers. It has no network-copyleft obligation. NCOSL remains pending legal review with no claim of OSI approval.

Cryptocurrency funding aims to establish liquidity and market adoption. It is separate from paying the founder or financing software development. LegacyBound should demonstrate practical utility before public promotion of NC. The project makes no promise of investment returns, appreciation or staking yields. Exchange integration, initial liquidity and regulatory review remain open. See [governance and adoption](docs/specs/GOVERNANCE.md).

## Dependencies and security review

- Monetary rules must distinguish minted NC held for payment from unminted pending claims. Variable redemption liabilities and simultaneous mints can exhaust headroom unless issuance, revaluation and settlement share a consistent accounting model.
- Staking entry burns reduce holdings permanently. Consensus designs that need slashable value must define separate collateral or establish an alternative security mechanism. Holdings-based charges also create incentives to split holdings and must be evaluated with validator selection.
- Repository revocation depends on verifiable and sufficiently fresh chain state. During a partition a supplier may have to deny a legitimate request to avoid serving a revoked principal. Encrypted contents still leave potential metadata and traffic leakage.
- Recovery and payout rules must coordinate so a compromised wallet cannot redirect pending earnings. The recovery authority itself needs protection against takeover, loss and conflicting requests.
- Consumer-node targets depend on realistic sync, archival availability and adversarial load measurements. Contribution recognition needs evidence and dispute rules that do not create a mandatory trusted gatekeeper.
- Automated coverage must exercise monetary invariants, unauthorized operations, replay, malformed data and failure boundaries before implementation is accepted. See the [security specification](docs/specs/SECURITY.md).
