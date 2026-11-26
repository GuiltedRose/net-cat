# NullCat technical whitepaper

**Draft v0.4.0**

## Purpose and scope

NullCat (NC) is an independent cryptocurrency and blockchain planned from scratch in Rust. NC is the chain's native currency. NullCat aims to support a decentralized economy in which open-source developers can receive compensation while retaining their software freedoms. LegacyBound.dev will use the chain for repository ownership, authorization and rewards. NullCat Wallet will provide non-custodial access. Blockchain, forge and wallet development belong in separate repositories.

This draft records approved architecture, provisional mathematical models, tunable simulation parameters and unresolved technical specifications. Supporting documents still reflect earlier decisions in several areas and will be synchronized separately.

## Monetary architecture and representation

NC is freely transferable and native to NullCat. WetCat (WC) is an account-bound, non-transferable reward asset. Neither has a fixed global supply cap. The approved genesis mint is **50,000,000 NC**. Distribution and validator bootstrapping remain open. Mainnet distribution must be public and independently verifiable. Deterministic issuance and burns govern monetary expansion. The design seeks gradual expansion, periods of deceleration and stabilization, and subsequent expansion. All native transaction fees burn permanently. There is no prefunded reward pool, privileged minting authority or automatic founder allocation.

| Parameter | NC | WC |
| --- | --- | --- |
| Balance representation | `u64` | `u64` |
| Decimal precision | 5 | 3 |
| Atomic units per coin | 100,000 | 1,000 |
| Balance storage | 8 bytes | 8 bytes |
| Aggregate accounting | `u128` | `u128` |

Consensus-critical calculations use deterministic fixed-point integer arithmetic with checked wide intermediates. Any operation whose intermediates exceed `u128` needs an explicitly specified safe arithmetic strategy. Reject overflowing transitions and prohibit integer wrapping and silent truncation. Representation limits are technical safety boundaries. Decimal constants in the simulation equations express mathematical values for integer implementation.

### WetCat and effective monetary supply

WC is automatically awarded for eligible developer contributions and automatically participates in value appreciation. It requires no staking entry burn. Consensus prohibits account and wallet transfers, direct purchase, exchange trading, deposits and withdrawals, decentralized exchange trading and bridging. There is no general-purpose WC transfer transaction. Partial and full redemption permanently destroy all WC selected and convert its redemption value into NC.

The approved initial conversion ratio is **5 WC : 1 NC**. The initial per-account accumulation limit is 1% of the genesis NC supply in NC redemption value. At genesis, `50,000,000 NC × 1% = 500,000 NC`, and `500,000 NC × 5 WC/NC = 2,500,000 WC`. The WC quantity is a derived starting value. The cap adjusts dynamically and applies to the base developer identity across wallet replacement, multiple associated wallets and wallet transfers. Future cap adjustments and how appreciation affects accounts at or above the limit remain open.

| Symbol | Meaning |
| --- | --- |
| `S` | Outstanding NC, including locked and staked NC |
| `W` | Outstanding WC |
| `R` | NC redemption value per WC |
| `L` | Outstanding WC redemption liabilities in NC |
| `T` | Effective monetary supply in NC |

```text
L = W × R
T = S + L
```

WC awards and appreciation create liabilities charged to the shared issuance budget. Redemption settles an existing liability. It moves an equivalent value from `L` into `S` and must create zero additional effective issuance. Settlement must reconcile any rounding differences and prevent double-counting NC already minted and held for payment and the unminted portion of monetary obligations. The appreciation index, scale conversions, transaction ordering, rounding and liability reconciliation remain unresolved.

## Reward architecture and provisional economic models

The equations and coefficients below are approved for simulation. Their constants are tunable before production adoption, and the technical details called out as open require specification.

### Annual gross issuance ceiling

```text
G = 50,000,000 NC
r(T) = 0.01 + 0.04 × G / max(G, T)
E_max(T) = T × r(T)

For T ≥ G:
E_max(T) = 0.01 × T + 2,000,000 NC
```

`G` is the genesis mint, `r(T)` is the maximum annual gross issuance rate and `E_max(T)` is the annual ceiling. The rate starts at 5% when `T = G`, remains 5% below `G` and approaches 1% as `T` grows. The ceiling limits gross issuance. It creates no obligation to issue. Deterministic reward-period budgets must avoid exceeding the annual intent through compounding, repeated recalculation or rounding. The reference supply, period accounting and budget reset rules remain open.

### Dynamic utilization

```text
E_actual = E_max × U
0 ≤ U ≤ 1
U_next = 0.95 × U_current + 0.05 × U_target
U_target = 0.40 × V + 0.40 × D + 0.20 × M

Q = S × q
V = min(1, A / Q)
```

`U` is issuance utilization, updated daily in the provisional model. The normalized inputs `V` (validator participation), `D` (qualified developer activity) and `M` (monetary stability) each lie in [0, 1]. `A` is eligible actively participating bonded NC and `Q` is target active bonded NC, with provisional `q = 10%`. Monetary stability must also constrain issuance independently of the weighted utilization target, so participation cannot override monetary safety limits. Raw transaction volume, commit counts and account creation cannot independently justify issuance. Daily timing, startup utilization, normalization, manipulation resistance, handling `Q = 0`, the independent safety constraint and deterministic rounding remain open.

### Shared reward budget

| Category | Provisional allocation |
| --- | ---: |
| Active validator rewards | 30% |
| WC contribution rewards | 30% |
| Passive NC staking rewards | 15% |
| WC value appreciation | 15% |
| Storage rewards | 10% |
| Total | 100% |

All monetary rewards draw from this budget, including the NC value of new WC liabilities. Active contributions and infrastructure should collectively receive more compensation than passive ownership. Unused allocations remain unissued and do not automatically move to other categories. No category may independently exceed the shared issuance budget. Validator reward scoring remains open.

### Progressive NC staking entry burn

Passive NC staking requires a permanent entry burn. A network baseline of **0.01%–2%** is separate from the participant concentration penalty. In this equation `M`, `P` and `C` are normalized monetary pressure, staking saturation and network concentration pressure. Monetary pressure here is distinct from monetary stability in the utilization equation.

```text
b_target = 0.0001 + 0.0199 × (0.5M + 0.3P + 0.2C)
b_network_next = 0.95 × b_network_current + 0.05 × b_target

h = H / S
b_individual = b_network + 0.30 × sqrt(h)
burned_amount = deposit × b_individual
staked_principal = deposit - burned_amount
```

The network baseline is smoothed daily. `H` is the participant's eligible NC holdings, including staked holdings. The provisional concentration coefficient is 0.30. Use a deterministic integer square root with specified fixed-point scaling. Individual rates can exceed 2% and must remain mathematically valid and below 100%. Apply the rate only to new deposits. Partial and full unstaking incur no new entry burn, and later deposits use the rate then applicable.

The definitions of the baseline inputs, initial baseline, observation timing, zero-supply handling and burn rounding remain open. Unrelated wallets cannot necessarily be attributed to one beneficial owner. Simulate wallet splitting and concentration attacks even where accounts can aggregate linked wallets.

### Passive NC staking rewards

```text
P_reward = 0.15 × E_actual
R_i = P_reward × P_i / P_total
```

`P_i` is participant `i`'s time-weighted eligible staked principal and `P_total` is the corresponding total. Time weighting limits reward-period deposit manipulation. With zero eligible participation, the allocation remains unissued. Percentage yield is an outcome of distribution. There is no independent fixed APY or separate issuance mechanism for yield. Settlement, compounding, rounding, minimum participation and withdrawal conditions remain open where unspecified.

### Developer contribution rewards

WC compensates qualified productive contributions. Imported Git history remains intact and generates no retroactive rewards. Eligible new contributions may earn WC after protocol activation and repository registration. Duplicate contributions cannot earn twice. Git timestamps alone cannot establish eligibility. Raw commit count is invalid as a valuation mechanism, including AI-generated commit volume. Founders receive the same treatment as other contributors. Contribution scoring, evidence, attribution, verification and disputes remain open. Ordinary Git operations require no blockchain transaction fee. Cryptographic CTF challenges remain outside the current scope.

### Storage rewards

Verifiable storage is an approved participation mechanism. Operators receive NC directly from the shared issuance budget using the provisional 10% allocation with no NC investment or consensus role required. Both eligible repository data and historical ledger archives qualify in principle. Rewards prioritize useful, retrievable data with insufficient replicas and discourage redundant storage, artificial farming and duplicate claims. Operators may prune without slashing or confiscation and stop receiving rewards for data they no longer retain.

```text
P_i = D_i × A_i × V_i × N_i
R_i = 0.10 × E_actual × P_i / sum(P_j)
```

For storage operator `i`, `D_i` is eligible verifiably stored data and `A_i`, `V_i` and `N_i` are availability, storage verification and network replication need coefficients. The allocation remains unissued if total eligible score is zero. Data eligibility, exact scoring and normalization, proof mechanisms, anti-farming protections and settlement rounding remain open.

### Native fees

Native transaction fees must stay extremely low. A standard transaction costing below one US cent is a design target. A fixed NC fee cannot guarantee that USD ceiling at all valuations. Congestion affects processing availability and confirmation time without increasing fees or enabling priority bidding. All native fees burn permanently. Fee calculation, spam controls, scheduling and any market-price adjustment remain open. See [native fees](docs/specs/FEES.md).

## Consensus architecture

NullCat uses permissionless Proof of Stake and rotating validator committees with Tendermint-style Byzantine Fault Tolerant finality. Committee selection is cryptographically randomized and stake-weighted. Finalization requires **greater than two-thirds** of committee voting power. Nodes independently verify transactions and state transitions. Passive staking is separate from active validation. Active validators maintain separate slashable bonded NC, with no founder-controlled or privileged class.

Occasional downtime costs rewards. Prolonged inactivity may remove eligibility, and provable malicious consensus behavior may result in slashing. Validator withdrawals require a suitable unbonding period. The combined design requires its own safety and liveness analysis covering Byzantine fault assumptions, locking, round changes, committee transitions, randomness manipulation, partitions and long-range attacks. Committee sampling and Tendermint-style voting alone do not establish those guarantees.

Committee size, voting-power assignment, selection and rotation mathematics, randomness and cryptographic primitives, collateral, unbonding periods and slashing percentages remain open. Resource requirements prioritize consumer hardware and residential internet over throughput.

## Ledger, pruning and networking

P2P operation has no mandatory central gateway. Wire and ledger data use compact deterministic binary serialization. The design takes inspiration from Solana's general approach to storage while targeting ordinary hardware and connectivity. Light clients, pruned full nodes, active validators, archival nodes, repository storage nodes and passive stakers are independent roles that may overlap.

Pruning is a native requirement. Supply authenticated finalized state snapshots and configurable historical retention above security minimums. Ordinary validators need no genesis-to-present archive. Preserve all active monetary and authorization state, required replay protection and consensus evidence. Pruning must preserve monetary state and authorization outcomes. Historical archival participation is optional and decentralized.

Repository storage has configurable limits and is pruned independently of the ledger. Inactivity must never automatically trigger network-wide deletion of the last available repository copy. Snapshot authentication, secure checkpointing, ledger serialization and state commitments, retention minima, active state growth, historical availability and how the network detects the last replica remain open. Resource budgets include initial sync, catch-up and steady-state participation.

## Repository registry and replication

LegacyBound will register public and private repositories on-chain. Signed records establish repository ownership and permission changes. Git contents stay off-chain and are distributed over P2P infrastructure that can operate independently of blockchain validation. Public contents require no retrieval authorization. Private contents are encrypted, and each supplier must verify explicit cryptographic authorization before transferring private data. Unauthorized nodes must receive no private repository contents.

Revocation prevents future authorized synchronization and cannot erase downloaded copies. Privacy-preserving metadata and permission representations remain research topics. Candidate approaches include commitments and proofs bound to the requesting party and the current authorized state. Authorization depends on how suppliers establish state freshness and finality during delays or partitions. Storage rewards provide replication incentives. Replication proofs, routing, key rotation and privacy leakage through network activity remain open.

Ordinary collaboration, including repository creation, cloning, pushing commits, issues, pull requests, code review and ordinary access, requires no cryptocurrency payment. LegacyBound will preserve ordinary Git client compatibility. Investigate gitoxide, libgit2 and system Git before selecting an implementation. Migration must preserve full history, commit hashes, branches, tags and signatures. How registry writes are funded while collaboration remains free is open.

## Developer identity and earnings

Developer accounts exist independently of individual wallet addresses and may have no linked wallets. Linking requires cryptographic proof of wallet ownership. Ordinary removal requires approval from both the account and the associated wallet. Wallets have no unilateral control over developer accounts. An independent recovery mechanism must replace compromised wallets while preserving repository ownership, contribution history and account-controlled funds. Financial keys and developer authorization keys should be isolated where practical.

Financial participation is optional. Users without eligible linked wallets may collaborate and do not accrue monetary participation rewards. Batch contribution evidence and settlement where practical. Reward meaningful work and avoid disproportionate rewards from raw activity volume, including AI-assisted activity. Pending earnings remain under on-chain account control until scheduled payout or redemption. Wallet replacement does not transfer repository ownership. Proposals include distinct account and wallet authorities and settlement records to prevent duplicate payouts. Contribution eligibility, attribution, valuation, payout cadence and recovery procedures remain open. Deterministic settlement depends on a verifiable rule for recognizing off-chain contribution evidence. See the [identity specification](docs/specs/IDENTITY.md).

## Wallets and signing

NullCat Wallet is a separate non-custodial project. The browser extension comes first, with possible native apps later. It will support NC and interact with BTC, ETH and SOL through their respective native networks without requiring bridges. NC will have a dedicated Ledger device application. Supported external currencies may use existing Ledger applications.

Transaction serialization must accommodate embedded hardware limits and clear signing. Candidate formats should give hardware signers enough context to show the operation, network, recipient, amount and fees as applicable. Private keys and recovery secrets must never reach network nodes. Signing formats and key conventions remain undecided. See [wallet integration](docs/specs/WALLET.md).

## Licensing, governance and adoption

LegacyBound.dev will become the canonical source host, with GitHub continuing as an independent public mirror for accessibility, transparency and redundancy. Migration preserves complete Git history. Blockchain, forge and wallet remain separate repositories. Forge Bounties is unrelated.

Founders and developers may hold ordinary NC wallets. The protocol prohibits privileged wallets, administrative minting, hidden allocations, developer taxes, universal recovery keys, arbitrary seizure, unilateral founder upgrades and administrative bypasses of monetary invariants. Development and integration testing must work without privileged monetary authority. Investigate reproducible builds, signed releases, independent audits and transparent upgrades. See [governance](docs/specs/GOVERNANCE.md) and [attack scenarios](docs/specs/SECURITY.md#attack-scenarios).

[BSD-3-Clause](LICENSE) remains active. The [NCOSL draft](LICENSE.draft.md) is the intended ecosystem default and will evolve in response to concrete development issues while preserving BSD-style software freedoms. It remains inactive pending professional legal review, documented rights-holder authorization and explicit approval. Hosted repository owners choose their own licenses. See the [contribution and licensing policy](README.md#licensing).

NC floats freely, with no fiat peg, guaranteed fiat redemption value or mandatory protocol-owned liquidity pool. External listings and liquidity remain independent of chain operation. The initial market strategy uses centralized exchanges to establish external NC trading and liquidity. Coinbase, Kraken, Binance and OKX are prospective targets. No listing or partnership is confirmed. LegacyBound should demonstrate practical utility before public promotion and formal listing applications. Native integration depends on documented node interfaces and independently verifiable balances, deposits, withdrawals and finality. Core blockchain operation must remain independent of exchanges, bridges and external financial services.

Future wallet features may include market-based NC-to-USDC conversion and settlement on Solana or another supported network. A native decentralized exchange is a possible long-term expansion. Cryptocurrency funding aims at liquidity and adoption, separate from founder compensation or software development financing. Public communications must avoid promises of returns, appreciation or staking yields. See [market integration](docs/specs/MARKET_INTEGRATION.md).

## Economic simulation and open specifications

Simulate all provisional models before production adoption using deterministic integer arithmetic that matches intended production behavior. Present coefficients are simulation baselines. Economic sustainability remains to be demonstrated.

- Simulate normal, rapid and negligible adoption over decades and centuries, minimal and excessive burns, low validator participation and reward allocation sustainability.
- Evaluate validator and passive staking concentration, wallet splitting and Sybil attacks, mass unstaking, simultaneous WC redemptions and rapid WC liability accumulation.
- Exercise contribution farming, storage farming, insufficient repository replication, malicious manipulation of utilization coefficients, monetary feedback oscillation and adjustment stability.
- Exercise integer representation limits, wide intermediate arithmetic and rounding at each monetary boundary.

The issuance ceiling's declining rate can coincide with a rising absolute gross budget. Stabilization depends on actual utilization, spending of allocations and burns. Positive feedback from staking, activity or appreciation needs simulation and a separately specified monetary safety constraint. Define whether the effective supply valuation represents only unsettled obligations and how rounding preserves redemption neutrality, including repeated partial redemptions. The model also needs an explicit policy for appreciation when an account crosses its accumulation limit without receiving new WC.

Implementation depends on the WC appreciation index, contribution valuation and dynamic account cap, monetary pressure and stability measures, initial utilization, annual-to-period accounting and validator scoring. Consensus still requires collateral and slashing mathematics, committee selection and rotation mathematics, cryptographic primitives, and genesis validator bootstrapping. Storage proofs, ledger serialization and state commitments, governance and upgrade authorization, and exact genesis distribution remain open.

Private repository revocation depends on fresh, verifiable chain state during partitions. Wallet recovery and payout routing must prevent redirection by a compromised wallet. Free collaboration needs registry admission and funding without a user cryptocurrency charge, and affordable native transactions need spam-resistant scheduling. Exchange integration depends on independently verifiable finality and reconciliation. Automated coverage must exercise monetary and authorization invariants, replay, malformed data and failure boundaries before implementation is accepted.
