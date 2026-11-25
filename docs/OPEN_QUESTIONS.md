# Open questions

All choices below require explicit approval. Dependencies identify decisions that must be coordinated. A prototype or draft must not silently become a protocol default.

| Area | Unresolved decisions and dependencies |
| --- | --- |
| Monetary units and genesis | Approve the proposed cap value, atomic precision, exact genesis amount and allocation. Precision and numeric width must accommodate the cap before encoding is fixed. |
| Issuance | Define growth and regenerative issuance formulas, inputs, rounding, eligibility, cap contention, pending NC claims and interaction with secondary liabilities. |
| Staking | Define eligible holdings, measurement window, dynamic entry burn, reward calculations and effectiveness against Sybil splitting and wealth concentration. Coordinate with consensus and issuance. |
| Secondary asset | Decide whether to adopt it. Determine name, account rules, issuance, valuation and conversion mathematics, accrual, redemption conditions and liability treatment. Settle which claims are guaranteed before selecting a reservation model. |
| Consensus | Select consensus and validators, define finality, validator collateral and penalties, and evaluate Sybil and whale resistance. Clarify interaction between irreversible entry burns and slashable collateral. |
| Rewards | Define contribution eligibility, evidence, attribution, valuation, disputes, duplicate prevention, payout cycle duration and wallet eligibility transitions. |
| Identity | Define account and wallet recovery, authority thresholds, challenge scope and conflict handling, and rules for existing pending earnings when no eligible wallet remains. |
| Ledger | Define transaction formats, signing domains, block sizes and intervals, state commitments, pruning and state synchronization, snapshot trust and light-client verification. |
| Networking | Set resource and bandwidth budgets, discovery, peer diversity and transport, then measure against residential connections and consumer hardware. |
| Repositories | Define replication incentives, data routing, private metadata representations, authorization proofs, finality and freshness checks, encryption and revocation key rotation. |
| Wallet | Choose NC key and address conventions, signing format and hardware limits, supported Ledger flows and client trust and recovery models per chain. |
| Governance | Define participation, upgrade and activation procedures, emergency response and recovery from faulty upgrades without standing insider privileges. |
| Adoption | Design exchange integration and initial liquidity, establish practical utility criteria and obtain regulatory review before public promotion or market activity. |
| License | Obtain professional review of final provisions, rights and notices, compensation language, disclaimer, patent treatment, compatibility and any approval process. BSD-3-Clause remains active pending review, rights-holder authorization and explicit adoption of NCOSL. |
