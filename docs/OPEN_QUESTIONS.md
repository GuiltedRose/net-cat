# Open questions

All choices below require explicit approval. Dependencies identify decisions that must be coordinated. A prototype or draft must not silently become a protocol default.

| Area | Unresolved decisions and dependencies |
| --- | --- |
| Genesis and NC accounting | Exact few-million NC mint and allocation, public verification procedures, lifetime issuance representation beyond `u64` and overflow behavior. The cap and NC precision are approved. |
| Issuance | Growth and regenerative issuance formulas, claim ordering, cap contention, guaranteed liabilities, settlement failure and capacity allocation between staking and WC. |
| NC staking | Eligible holdings, measurement window, dynamic entry burn, reward allocation and denomination, collateral, validator relationship, staking duration, withdrawal conditions and Sybil and whale resistance. |
| WetCat | WC precision, per-account cap, issuance and automatic yield, conversion mathematics, ratification of the proposed 5 WC to 1 NC ratio, global index feasibility, redemption rounding, eligibility, and backing or reservation through index changes. |
| Fees | Fee calculation, standard transaction class and USD measurement basis, scheduling, spam resistance, possible market-price adjustment and who funds registry writes for free collaboration. |
| Consensus | Consensus and validator selection, finality, collateral, penalties and resistance to Sybil splitting and concentration. |
| Rewards | Contribution eligibility, evidence and valuation, meaningful work and AI-assisted activity, disputes, duplicate prevention, batching, payout cycle and wallet eligibility transitions. |
| Identity | Account and wallet recovery, authority thresholds, challenges and conflict handling, and treatment of existing earnings when no eligible wallet remains. |
| Ledger | Transaction and block formats, signing domains, block sizes and intervals, commitments, pruning, state synchronization, snapshot trust and light-client verification. |
| Networking | Resource and bandwidth budgets, discovery, peer diversity, transport and validation on consumer hardware and residential links. |
| Repositories | Evaluate gitoxide, libgit2 and system Git, verify history migration, define replication incentives, encrypted data routing, private metadata and proof formats, finality and freshness, and key rotation. |
| Wallet | NC keys and addresses, signing format and hardware limits, Ledger flows, chain-specific verification and recovery, and possible NC-to-USDC settlement. |
| Governance and security | Upgrade and activation procedures, capture resistance, emergency response, reproducible builds, release signatures and independent audits. |
| Market | Practical utility criteria, native exchange interfaces and reconciliation, initial liquidity, listing process and regulatory review. |
| License | Final NCOSL provisions, professional legal review, rights-holder authorization and explicit adoption. BSD-3-Clause remains active and hosted owners choose their own licenses. |
