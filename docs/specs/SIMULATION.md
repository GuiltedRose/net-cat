# Economic simulation requirements

The future simulator must reproduce intended consensus arithmetic with deterministic fixed-point integers, checked intermediates, specified rounding and overflow rejection. Production period accounting, arithmetic scales and unsettled protocol formulas require explicit specification. Results must be reproducible from the scenario and parameter set. The [whitepaper](../../WHITEPAPER.md) and [monetary protocol](MONETARY_PROTOCOL.md) supply provisional baselines.

## Independently configurable inputs

- Initial and long-term gross issuance ceiling parameters, utilization weights and initial utilization.
- Monetary smoothing coefficient and daily timing, validator target bonding ratio and reward allocation percentages.
- Baseline entry-burn boundaries, burn smoothing and holdings concentration coefficient.
- Future WC appreciation and cap parameters and storage scoring parameters once approved. In allocation experiments preserve the total budget and report unused amounts.

## Scenarios

- Normal, rapid and negligible adoption over decades and centuries, minimal and excessive burning, low validator participation and reward allocation sustainability.
- Validator and passive stake concentration, wallet splitting, Sybil attacks and mass unstaking.
- Simultaneous WC redemptions, repeated partial redemptions and rapid WC liability accumulation.
- Contribution and storage farming, artificial replication demand and insufficient repository replication.
- Monetary feedback oscillation, adjustment stability and adversarial manipulation of utilization inputs and reward-period timing.
- Integer representation limits, intermediate overflow, scale conversion and rounding boundaries.

## Measurements

Track outstanding NC and WC, WC redemption liabilities, effective monetary supply, gross issuance, actual NC minting, total NC burning and net monetary expansion. Report reward distribution and unused allocations, staking participation, validator concentration, developer compensation and storage availability. Record arithmetic safety outcomes, redemption reconciliation, feedback stability and sensitivity to manipulation.
