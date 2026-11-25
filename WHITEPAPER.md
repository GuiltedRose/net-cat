# NullCat technical whitepaper

**Draft v0.3.0**

## Purpose and scope

NullCat (NC) is an independent cryptocurrency and blockchain planned from scratch in Rust. NC is the chain's native currency. NullCat aims to support a decentralized economy in which open-source developers can receive compensation while retaining their software freedoms. LegacyBound.dev will use the chain for repository ownership, authorization and rewards. NullCat Wallet will provide non-custodial access. Blockchain, forge and wallet development belong in separate repositories.

The requirements below constrain the design. Proposed mechanisms require approval, and the [open-questions register](docs/OPEN_QUESTIONS.md) tracks unresolved choices. Approved requirements, design targets and proposals have distinct status. A target guides design and a proposal requires ratification.

## Monetary accounting

The approved cap is **100 trillion NC outstanding**. With five decimal places, one NC contains 100,000 atomic units and the smallest unit is 0.00001 NC. The cap is 10,000,000,000,000,000,000 atomic units. Five is the highest precision that lets the full cap fit in an eight-byte `u64` balance. Consensus-critical monetary operations use exact integers with checked `u128` intermediates where needed. Cumulative lifetime issuance must be representable beyond `u64`. Genesis should mint a few million NC, with the exact amount and allocation open. The mainnet distribution must be publicly disclosed and independently verifiable.

Burns free headroom for future minting, so cumulative issuance can exceed the cap while outstanding supply remains bounded. NC can be held, transferred and traded independently of LegacyBound. Rewards follow deterministic protocol minting rules, with no dedicated prefunded pool, administrative mint, developer taxes, privileged wallets, hidden allocations or automatic founder rewards.

### Voluntary NC staking

Holders can stake raw NC independently of earning WC. Entry irreversibly burns a dynamic proportion of eligible holdings based on those holdings and network monetary conditions. The mechanism must discourage wealthy participants from monopolizing future issuance, and founders, developers and investors stake under the ordinary rules. Staking and WC accrual must coexist without either exhausting issuance capacity. Burn formula, reward allocation and denomination, collateral, validator relationship, duration and withdrawal conditions remain open. See the [monetary protocol](docs/specs/MONETARY_PROTOCOL.md).

### WetCat

WetCat (WC) is an approved account-bound asset earned through eligible LegacyBound participation. It automatically accrues redeemable NC value under protocol rules. WC has no independently fixed global cap and has a mandatory per-developer-account balance limit that survives changes in linked wallets. Consensus prohibits direct purchase, wallet transfers, exchange deposits and withdrawals, decentralized exchange trading and bridging. There is no general-purpose WC transfer transaction.

Partial and full redemption burn all WC selected and mint the corresponding NC at the applicable rate as one settlement. Guaranteed redemption and yield require sufficient backing or reserved issuance capacity within the NC cap. Investigate a global conversion index to accrue value without rewriting every account. **5 WC to 1 NC** is a proposed initial ratio pending ratification. WC precision, account cap, issuance, yield, conversion math and redemption rounding remain open. See [WetCat](docs/specs/WETCAT.md#open-decisions).

### Native fees

Native transaction fees must stay extremely low. A standard transaction costing below one US cent is a design target. A fixed NC fee cannot guarantee that USD ceiling at all valuations. Congestion affects processing availability and confirmation time without increasing fees or enabling priority bidding. All native fees burn permanently. Fee calculation, spam controls, scheduling and any market-price adjustment remain open. See [native fees](docs/specs/FEES.md).

## Consensus, ledger and network

P2P networking is fundamental. Independent operation must be possible without a mandatory central gateway or authority. Consumer hardware and ordinary residential links should support meaningful participation. The ledger and network should remain lightweight, avoiding Solana-scale hardware and bandwidth demands. Compact, deterministic binary serialization is required.

Candidate consensus protocols must be evaluated for safety, finality, validator selection, resource demand, adversarial participation and recovery from partitions. Consensus, collateral, penalties, block size and timing remain undecided. Proposed ledger designs include authenticated snapshots for state sync, pruning, light clients and independently operated archival nodes. Authenticated synchronization depends on a verifiable state commitment and a way to establish its consensus status. Resource budgets must include initial sync and catch-up as well as steady-state participation. See [consensus](docs/specs/CONSENSUS.md), [ledger](docs/specs/LEDGER.md) and [networking](docs/specs/NETWORKING.md).

## Repository registry and replication

LegacyBound will register public and private repositories on-chain. Signed records establish repository ownership and permission changes. Git contents stay off-chain and are distributed over P2P infrastructure that can operate independently of blockchain validation. Public contents require no retrieval authorization. Private contents are encrypted, and each supplier must verify explicit cryptographic authorization before transferring private data. Unauthorized nodes must receive no private repository contents.

Revocation prevents future authorized synchronization and cannot erase downloaded copies. Privacy-preserving metadata and permission representations remain research topics. Candidate approaches include commitments and proofs bound to the requesting party and the current authorized state. Authorization depends on how suppliers establish state freshness and finality during delays or partitions. Replication incentives, routing, key rotation and privacy leakage through network activity remain open. See the [repository specification](docs/specs/REPOSITORIES.md).

Ordinary collaboration, including repository creation, cloning, pushing commits, issues, pull requests, code review and ordinary access, requires no cryptocurrency payment. LegacyBound will preserve ordinary Git client compatibility. Investigate gitoxide, libgit2 and system Git before selecting an implementation. Migration must preserve full history, commit hashes, branches, tags and signatures. How registry writes are funded while collaboration remains free is open.

## Developer identity and earnings

Developer accounts exist independently of individual wallet addresses and may have no linked wallets. Linking requires cryptographic proof of wallet ownership. Ordinary removal requires approval from both the account and the associated wallet. Wallets have no unilateral control over developer accounts. An independent recovery mechanism must replace compromised wallets while preserving repository ownership, contribution history and account-controlled funds. Financial keys and developer authorization keys should be isolated where practical.

Financial participation is optional. Users without eligible linked wallets may collaborate and do not accrue monetary participation rewards. Batch contribution evidence and settlement where practical. Reward meaningful work and avoid disproportionate rewards from raw activity volume, including AI-assisted activity. Pending earnings remain under on-chain account control until scheduled payout or redemption. Wallet replacement does not transfer repository ownership. Proposals include distinct account and wallet authorities and settlement records to prevent duplicate payouts. Contribution eligibility, attribution, valuation, payout cadence and recovery procedures remain open. Deterministic settlement depends on a verifiable rule for recognizing off-chain contribution evidence. See [identity](docs/specs/IDENTITY.md) and [rewards](docs/specs/REWARDS.md).

## Wallets and signing

NullCat Wallet is a separate non-custodial project. The browser extension comes first, with possible native apps later. It will support NC and interact with BTC, ETH and SOL through their respective native networks without requiring bridges. NC will have a dedicated Ledger device application. Supported external currencies may use existing Ledger applications.

Transaction serialization must accommodate embedded hardware limits and clear signing. Candidate formats should give hardware signers enough context to show the operation, network, recipient, amount and fees as applicable. Private keys and recovery secrets must never reach network nodes. Signing formats and key conventions remain undecided. See [wallet integration](docs/specs/WALLET.md).

## Licensing, governance and adoption

LegacyBound.dev will become the canonical source host, with GitHub continuing as an independent public mirror for accessibility, transparency and redundancy. Migration preserves complete Git history. Blockchain, forge and wallet remain separate repositories. Forge Bounties is unrelated.

Founders and developers may hold ordinary NC wallets. The protocol prohibits privileged wallets, administrative minting, hidden allocations, developer taxes, universal recovery keys, arbitrary seizure, unilateral founder upgrades and administrative bypasses of monetary invariants. Development and integration testing must work without privileged monetary authority. Investigate reproducible builds, signed releases, independent audits and transparent upgrades. See [governance](docs/specs/GOVERNANCE.md) and [attack scenarios](docs/specs/SECURITY.md#attack-scenarios).

[BSD-3-Clause](LICENSE) remains active. The [NCOSL draft](LICENSE.draft.md) is the intended ecosystem default and will evolve in response to concrete development issues while preserving BSD-style software freedoms. It remains pending professional legal review, documented rights-holder authorization and explicit approval. Hosted repository owners choose their own licenses. See the [contribution and licensing policy](README.md#licensing).

The initial market strategy uses centralized exchanges to establish external NC trading and liquidity. Coinbase, Kraken, Binance and OKX are prospective targets. No listing or partnership is confirmed. LegacyBound should demonstrate practical utility before public promotion and formal listing applications. Native integration depends on documented node interfaces and independently verifiable balances, deposits, withdrawals and finality. Core blockchain operation must remain independent of exchanges, bridges and external financial services.

Future wallet features may include market-based NC-to-USDC conversion and settlement on Solana or another supported network. A native decentralized exchange is a possible long-term expansion. Cryptocurrency funding aims at liquidity and adoption, separate from founder compensation or software development financing. Public communications must avoid promises of returns, appreciation or staking yields. See [market integration](docs/specs/MARKET_INTEGRATION.md).

## Dependencies and security review

- Monetary rules must distinguish minted NC held for payment from unminted pending claims. Variable redemption liabilities and simultaneous mints can exhaust headroom unless issuance, revaluation and settlement share a consistent accounting model.
- Staking entry burns reduce holdings permanently. Consensus designs that need slashable value must define separate collateral or establish an alternative security mechanism. Holdings-based charges also create incentives to split holdings and must be evaluated with validator selection.
- Repository revocation depends on verifiable and sufficiently fresh chain state. During a partition a supplier may have to deny a legitimate request to avoid serving a revoked principal. Encrypted contents still leave potential metadata and traffic leakage.
- Recovery and payout rules must coordinate so a compromised wallet cannot redirect pending earnings. The recovery authority itself needs protection against takeover, loss and conflicting requests.
- Consumer-node targets depend on realistic sync, archival availability and adversarial load measurements. Contribution recognition needs evidence and dispute rules that do not create a mandatory trusted gatekeeper.
- Free collaboration needs a way to pay for or otherwise admit registry writes without passing a cryptocurrency charge to users. Low fees without congestion pricing also require spam-resistant scheduling.
- Exchange integration depends on independently verifiable finality and documented reconciliation behavior.
- Automated coverage must exercise monetary invariants, unauthorized operations, replay, malformed data and failure boundaries before implementation is accepted. See the [security specification](docs/specs/SECURITY.md).
