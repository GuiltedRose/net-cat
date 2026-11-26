# Architectural decision log

Permanent record of established project constraints. Append subsequent approvals with rationale, affected specifications and superseded entries. Proposed mechanisms appear in the specifications and are tracked in [open questions](OPEN_QUESTIONS.md).

| ID | Established direction | Rationale |
| --- | --- | --- |
| D-001 | Native Rust chain, separate forge and wallet repositories | Keep monetary infrastructure independent of application operators and wallet clients. |
| D-002 | Hard limit on outstanding NC with a proposed ceiling of 100 trillion NC, fee burns and deterministic reward minting | Bound monetary supply while allowing future issuance after burns. |
| D-003 | Dynamic staking entry burn and participant-neutral monetary rules | Preserve earning access and avoid privileged developer and founder claims. |
| D-004 | P2P operation, compact encoding and consumer-hardware participation | Reduce barriers to independent validation and data distribution. |
| D-005 | On-chain repository registry and independent off-chain Git storage, with supplier-side private authorization | Verify control changes without storing Git objects on-chain. |
| D-006 | Developer accounts independent of optional wallets, with account-level claims and independent recovery | Preserve ownership and earnings across wallet loss or compromise. |
| D-007 | Browser-first non-custodial wallet, native chain support and eventual Ledger signing | Keep secrets local and external currency activity on its native network. |
| D-008 | Permissive licensing objective and governance without automatic insider privileges | Preserve software freedoms and equal protocol treatment. |
| D-009 | Demonstrate forge utility before public promotion and pursue liquidity for adoption | Separate market adoption from founder compensation and development finance. |
| D-010 | Documentation and legal drafting before implementation | Review dependencies and approve relevant specifications before committing to protocol behavior. |
| D-011 | BSD-3-Clause active, with NCOSL pending professional review, rights-holder authorization and explicit adoption | Establish standard permissive terms now and preserve contributor control over any future relicensing. |
| D-012 | Ratify the 100 trillion NC cap and five decimal places with `u64` balances and checked `u128` intermediates | Supersedes D-002's provisional cap and v0.2.0's open NC precision. Five is the maximum precision that fits the whole cap in eight bytes. Lifetime issuance must be representable beyond `u64`. |
| D-013 | Adopt WetCat as the consensus-restricted account-bound asset | Supersedes v0.2.0's optional unnamed asset proposal. The proposed 5 WC to 1 NC initial ratio remains pending. |
| D-014 | Separate voluntary NC staking from WC earnings and require issuance capacity for both | Extends D-003 while keeping staking mechanics open. |
| D-015 | Require low burned fees with no congestion price or priority bids and free ordinary collaboration | Extends D-002. The sub-cent standard transaction goal is a target. |
| D-016 | Preserve Git interoperability and complete migration history. Measure meaningful work and batch reward evidence and settlement where practical | Extends D-005 and reduces unnecessary writes and activity-based reward gaming. |
| D-017 | Prioritize native centralized-exchange integration for initial market access | Refines D-009. Exchange targets are prospective and operation remains independent of external services. |
| D-018 | Prohibit administrative monetary bypasses and unilateral upgrades, require verifiable genesis and test without privileged authority | Extends D-003 and D-008 to reduce insider abuse. |
| D-019 | Make LegacyBound the intended canonical source host and retain GitHub as an independent public mirror | Supersedes v0.2.0's characterization of GitHub as temporary infrastructure. |
| D-020 | Retain BSD-3-Clause and intend NCOSL as the future ecosystem default subject to review and authorization | Extends D-011. Hosted repositories keep freedom of license choice. |
| D-021 | Approve uncapped NC and WC monetary architecture and a 50 million NC genesis mint | Supersedes the fixed-cap provisions of D-002 and D-012 and the earlier provisional genesis amount. Preserves permanent fee burns and no privileged issuance. Genesis distribution and validator bootstrapping remain open. |
| D-022 | Use five NC and three WC decimals, `u64` balances, `u128` aggregates and checked fixed-point arithmetic | Extends D-012's representation requirements. Integer capacity is a technical safety boundary. Wider intermediate strategies and rounding remain open. |
| D-023 | Approve initial 5 WC : 1 NC conversion and a dynamic base-account limit initially equal to 1% of genesis NC in redemption value | Supersedes D-013's pending ratio. WC awards and appreciation consume the shared issuance budget through liabilities. Redemption settles liabilities and replaces earlier fixed-cap reservation assumptions. |
| D-024 | Adopt one shared issuance budget and approve the ceiling, utilization, staking burn and proportional reward models for simulation | Extends D-003 and D-014. Provisional coefficients and allocations are tunable. Reward-period accounting, input metrics and production formulas require approval. |
| D-025 | Approve permissionless PoS with randomized stake-weighted rotating committees, Tendermint-style BFT finality and separate validator bonds | Supersedes the earlier open consensus-family choice. Committee mechanics, security assumptions, primitives and collateral and penalty parameters remain open. |
| D-026 | Require native pruning, authenticated finalized snapshots and independently operated storage roles | Replaces the earlier treatment of pruning as a candidate mechanism. Retention and checkpoint mechanics, state commitments and snapshot encoding remain open. |
| D-027 | Approve verifiable repository and ledger archival storage rewards paid in NC from the shared budget | Extends D-005 and D-024. The 10% allocation and scoring model are provisional. Proofs, replication requirements and anti-farming mathematics remain open. |
| D-028 | Approve contribution eligibility boundaries and preserve a freely floating NC market | Extends D-016 and D-017. Imports do not earn retroactive rewards. Market liquidity and listings remain independent of blockchain operation. |

The superseding entries above apply only to the identified provisions. Approved architecture governs subsequent design. Provisional models establish simulation baselines, their coefficients are tunable, and unresolved technical specifications still require explicit approval. See [whitepaper v0.4.0](../WHITEPAPER.md) and [simulation requirements](specs/SIMULATION.md).

## v0.5.0 decisions

| ID | Status and decision | Relationship to earlier decisions |
| --- | --- | --- |
| D-029 | Approved genesis state contains exactly 50 million NC and zero WC. Genesis is the sole exceptional mint. | Clarifies D-021. Recipients, distribution, liquidity and production bootstrap are deferred to launch. |
| D-030 | Approve provisional normalization, daily smoothing, category distribution and the effective supply invariant. | Extends D-024. Measurement, period accounting and rounding are unresolved. Coefficients are tunable. |
| D-031 | Approve provisional WC index, dynamic cap, cap-crossing treatment and contribution scoring and conversion. | Completes the baseline mechanisms left open in D-023 and extends D-028. Production evidence, timing and numerical policies remain open. |
| D-032 | Extend progressive burns to new validator bonds. Permit continuously locked stake to change roles without a second burn. | Refines D-003 and D-024. The post-burn bond minimum and withdrawal delays are provisional. |
| D-033 | Approve provisional committee, voting, epoch, MVB, validator reward and penalty models. | Refines D-025. Handover, secure randomness, evidence and time semantics need specification. |
| D-034 | Select cryptographic and compression benchmark candidates with distinct key roles and no custom primitives. | Extends D-004 and D-025. The suite and exact ciphersuites require validation. |
| D-035 | Approve canonical binary encoding constraints and specialized transaction families. Select preliminary layouts, sparse Merkle commitments and snapshot profiles. | Extends D-004 and D-026. Layouts and engineering parameters are provisional. |
| D-036 | Adopt replication-aware pruning and migration safety requirements. | Extends D-026 and D-027. Replica thresholds are tunable, and proofs and independence remain unresolved. |
| D-037 | Select rust-libp2p and modular transport and discovery candidates with prioritized traffic and independent bootstrap paths. | Extends D-004. Limits and stack selections are subject to benchmark and security review. |
| D-038 | Adopt independent passive staker and active validator governance chambers. | Extends D-008 and D-018. Thresholds and periods are provisional. Upgrade execution and emergency rules remain unresolved. |
| D-039 | Require integrated simulation and separate engineering benchmarks. | Extends D-024 through D-027. Synthetic scores are economic inputs and confer no production assurance. |
