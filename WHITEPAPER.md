# NullCat technical whitepaper

**Draft v0.5.0**

## Purpose and ecosystem architecture

NullCat is an independent Rust blockchain with a freely transferable native currency, NC. LegacyBound.dev is a decentralized Git collaboration platform and the intended canonical source host, with GitHub retained as an independent public mirror. NullCat Wallet is a separate non-custodial multichain wallet. The three projects use separate repositories. Forge Bounties is unrelated.

NullCat prioritizes independent operation on consumer hardware and residential connections, low CPU, memory, disk and bandwidth usage, long-term monetary sustainability and compensation for productive participation. Throughput is secondary. Founders, developers, investors, exchanges and infrastructure providers receive no privileged protocol authority. The specifications distinguish approved architecture, provisional mathematical models and tunable coefficients, engineering candidates, and unresolved production and launch decisions.

## Monetary protocol

NC has five decimal places and 100,000 atomic units per coin. WetCat (WC) has three decimal places and 1,000 atomic units per coin. Each balance occupies eight bytes as a `u64`, with `u128` aggregates. Neither asset has a fixed global supply cap. Consensus uses deterministic fixed-point integers, explicit rounding, checked wide intermediates and deterministic integer square roots. Arithmetic beyond `u128` intermediates needs an explicitly safe strategy, and overflowing transitions are rejected.

Outstanding NC `S` includes locked and bonded balances. With outstanding WC `W` and NC redemption value per WC `R`, liabilities are `L = W × R` and effective supply is `T = S + L`. Gross issuance includes new NC rewards and new WC liabilities. Redemption destroys WC and settles an equal liability into NC without independent effective issuance. The period invariant is `T_next = T_current + E - B`, where new effective issuance `E` is bounded by its budget and `B` is permanent monetary destruction. Native transaction fees are permanently burned. Sub-cent fees are a design target without congestion pricing or priority bidding.

At the simulation baseline the annual gross issuance ceiling starts at 5% and approaches 1% as effective supply grows. It limits issuance without requiring it. Daily utilization adjusts at a provisional 5% smoothing rate, targeting 40% validator participation, 40% qualified developer activity and 20% monetary stability. Generic bounded normalization uses nonnegative measurements relative to a target, and stability is one minus monetary pressure. An independent stability constraint is required. Gradual expansion, deceleration and stabilization are design objectives. The smoothing rule gives no guarantee of recurring cycles. See [monetary equations and budgets](docs/specs/MONETARY_PROTOCOL.md) and [native fees](docs/specs/FEES.md).

## Reward mechanisms

A single budget provisionally allocates 30% to active validators, 30% to WC contribution awards, 15% to passive NC staking, 15% to WC appreciation and 10% to storage. Unused allocations remain unissued. Eligible scores divide category budgets proportionally. Yield emerges from distribution without a fixed APY or separate inflation mechanism.

New passive deposits and newly funded validator bonds incur a progressive permanent burn. The provisional network baseline ranges from 0.01% to 2%, and the independent holdings penalty can raise the individual rate above 2%, with the total below 100%. Continuously locked passive stake can become validator collateral without a second burn. Passive staking pays by time-weighted eligible principal, requires no node or validator minimum, grants no consensus votes and is outside validator slashing. Partial and complete withdrawals incur no entry burn.

WetCat is earned for eligible contributions, is bound to the developer account, and cannot be purchased, transferred, traded, bridged or used for consensus or governance votes. Partial or full redemption permanently destroys selected WC in the one-way conversion to NC. The initial ratio is 5 WC to 1 NC. Appreciation applies the category budget to a global redemption index before new awards. The daily account cap uses effective supply times an initially 1% coefficient divided by the current redemption rate, giving an initial 500,000 NC redemption value or 2,500,000 WC. Above-cap balances keep appreciating without confiscation or forced redemption. Further awards wait for capacity. Wallet replacement and stacking do not change the base-account limit.

Contribution scores combine acceptance, verification confidence and impact. Awards convert to WC within remaining capacity. Verifiable useful repository storage and historical archives can earn NC directly without a bond. Storage scores weight eligible data, availability, verification and replication need. Empty capacity and artificial duplication earn nothing. See [rewards](docs/specs/REWARDS.md), [WetCat](docs/specs/WETCAT.md) and [storage](docs/specs/STORAGE.md).

## Consensus

Permissionless Proof of Stake uses rotating committees and Tendermint-style Byzantine fault tolerant finality. All nodes independently verify transactions and transitions. The initial model samples stake-weighted seats independently with replacement. Each seat carries one vote, including repeated seats held by the same validator. Finalization needs greater than two-thirds of committee voting power. The current committee finalizes the state authorizing its successor. A stall grants no unilateral replacement authority.

Validator eligibility requires the post-burn bond minimum, initially 0.001% of outstanding NC or 500 NC at genesis. Activation is voluntary and explicitly accepts signing, locking, slashing and unbonding obligations. The same NC cannot earn both passive and validator rewards simultaneously. The provisional committee rule targets 64–256 seats subject to the active population. The initial targets are ten-second blocks and rotations every 360 blocks. Validator rewards weight assigned seats by verified participation. Occasional downtime misses rewards, prolonged inactivity may remove eligibility, and independently verified malicious behavior can burn collateral. Provisional delays are one day for passive withdrawals and 30 days for bonded collateral, which stays slashable throughout unbonding. WC has no staking withdrawal delay. The [consensus specification](docs/specs/CONSENSUS.md) gives exact formulas, thresholds, penalties and the conditional binomial risk model.

## Cryptographic primitives

The engineering baseline selects BLAKE3-256 for hashing and BLAKE3 Merkle commitments, Ed25519 for wallet and validator signatures, ECVRF-Edwards25519 as a randomness proof candidate, and X25519, ChaCha20-Poly1305 and HKDF-SHA256 for repository key agreement, encryption and derivation. Zstandard is the historical compression baseline and Git uses native packfiles. BLS12-381 is a validator aggregation benchmark candidate. Keep key roles separate and define domain separation. These candidates require benchmarking and security analysis. See [cryptography](docs/specs/CRYPTOGRAPHY.md).

## Ledger and authenticated state

The versioned canonical binary format uses little-endian integers, fixed-width monetary fields, compact typed records and raw cryptographic fields. Human-readable APIs can emit decimal strings for exact amounts. Specialized transaction families cover monetary, identity, repository, storage and governance operations without requiring a general-purpose VM. The proposed header and NC transfer payload are 156 and 153 bytes respectively before applicable extra framing and context.

Provisional compressed sparse Merkle trees commit separate monetary, WC, staking, consensus, developer identity, repository authorization and governance namespaces into one 32-byte root. Finalized state updates must be atomic and canonical commitments independent of the database engine. See [ledger layouts and state](docs/specs/LEDGER.md).

## Historical storage and snapshots

Historical full nodes, pruned full nodes, light clients, validators, repository storage nodes and passive stakers are independently operable roles. Either full node mode may validate. Snapshot sync starts with an authenticated recent checkpoint, verifies compressed chunks against finalized state and independently validates subsequent blocks. The provisional baseline is daily snapshots, 4 MiB uncompressed chunks and two local snapshots. A root alone does not authenticate chain history.

Pruning is local and preserves active state, authorization, replay protection and required evidence. The archival target is five copies and automated pruning requires at least three other verified copies. Insufficient replication suspends affected automatic pruning. Git storage and pruning are separate, and inactivity alone cannot destroy the last repository copy network-wide. Validator infrastructure migration requires preservation of signing-safety state and prevention of simultaneous signing. See [ledger](docs/specs/LEDGER.md) and [storage](docs/specs/STORAGE.md).

## P2P networking

The provisional rust-libp2p stack uses QUIC with TCP, Noise and Yamux fallback. It supports independent bootstrap sources, decentralized discovery, residential NAT traversal where possible and replaceable infrastructure. Traffic separation protects consensus from bulk ledger and repository work. Operators control repository bandwidth and storage budgets. The ordinary peer target is 16 with a provisional maximum of 64 and no minimum. The provisional block payload limit is 256 KiB, with certificate and framing scope open. See [networking](docs/specs/NETWORKING.md).

## Repository and identity architecture

Git contents stay off-chain and remain compatible with ordinary Git clients. Signed registry records establish ownership and permissions. Public repositories are publicly retrievable. Private contents are encrypted and suppliers verify authorization against fresh finalized state before any data transfer. Revocation blocks future authorized sync and cannot erase earlier copies. Ordinary Git collaboration requires no cryptocurrency payment, and migration preserves commit hashes, branches, tags and signatures. Imported history earns no retroactive reward. Duplicate work, raw commit counts, Git timestamps alone, artificial AI activity and founder status cannot justify compensation.

Developer accounts persist independently of optional wallets. Linking proves wallet ownership, and ordinary removal requires both parties. Independent recovery preserves repository ownership, contribution history, WC limits and account-controlled earnings. Wallets lack unilateral authority. Accounts without an eligible associated wallet may collaborate and do not accrue monetary participation rewards. Recovery and private key management details remain open. See [repositories](docs/specs/REPOSITORIES.md) and [identity](docs/specs/IDENTITY.md).

## Wallet integration

NullCat Wallet starts as a non-custodial browser extension with possible later native apps. NC, BTC, ETH and SOL interact with their native networks. NC has a planned dedicated Ledger application. Signing must support clear confirmation of network, operation, recipient, amounts and fees. Private keys and recovery secrets remain local. See [wallet integration](docs/specs/WALLET.md).

## Governance

Major protocol amendments require independent approval from a chamber of eligible passive NC stakers and a chamber of all eligible active validators. Voting power follows committed or bonded NC respectively. There is no WC voting, separate governance token, founder veto or unilateral administrative upgrade. A finalized eligibility snapshot and lock prevent the same NC from counting in both chambers. The provisional threshold is greater than two-thirds approval with 50% minimum participation per chamber, a 14-day review, seven-day vote and 14-day activation delay. Quorum arithmetic, activation and safeguards against capture or deadlock need specification. See [governance](docs/specs/GOVERNANCE.md).

## Genesis requirements

Exactly 50 million NC and zero WC exist in genesis state. Genesis is the sole exceptional initialization mint. All subsequent issuance is deterministic. Production recipients, distribution, initial liquidity and validator bootstrapping are launch decisions. Simulations can use hypothetical allocations. See [genesis](docs/specs/GENESIS.md).

## Market integration and licensing

NC floats freely, with no fiat peg, guaranteed fiat redemption, investment returns or required protocol-owned liquidity pool. Coinbase, Kraken, Binance and OKX are prospects only. Listings and liquidity remain independent of chain operation. Initial cryptocurrency funding targets liquidity rather than founder compensation or development. LegacyBound should demonstrate utility before public promotion or listing applications. Wallet conversion and a native decentralized exchange are optional expansions. See [market integration](docs/specs/MARKET_INTEGRATION.md).

[BSD-3-Clause](LICENSE) is active. The [NCOSL draft](LICENSE.draft.md) remains inactive pending professional legal review, explicit approval and documented rights-holder authorization. Hosted projects choose their own licenses.

## Simulation and security validation

Integrated scenarios combine monetary feedback, WC liabilities and redemptions, staking and validator concentration, unreliable storage and infrastructure, committee and governance capture, and constrained synchronization. Hypothetical genesis allocations and synthetic contribution or storage scores support economic simulation. Separate engineering benchmarks and protocol-specific security analysis are required. No provisional parameter has been validated by this revision. See [simulation](docs/specs/SIMULATION.md) and [security](docs/specs/SECURITY.md).

## Remaining technical specifications

Production still requires deterministic budget and time accounting, measurement and rounding rules, verifiable participation and storage proofs, committee and checkpoint security, canonical encodings, upgrade authorization and wallet recovery. Benchmark assumptions and launch decisions have separate gates. See the [open-questions register](docs/OPEN_QUESTIONS.md).
