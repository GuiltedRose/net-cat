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

## Simulation requirements

Integrated simulation covers the complete baseline architecture.

The simulator must support configurable hypothetical genesis allocations without requiring actual production recipients.

Required scenarios include:

- Normal adoption.
- Explosive adoption.
- Negligible adoption.
- Decades and centuries of monetary operation.
- High and low burning.
- Monetary feedback oscillation.
- Rapid WC liability accumulation.
- Simultaneous WC redemption.
- Mass unstaking.
- Wallet splitting.
- Whale concentration.
- Contribution farming.
- Storage farming.
- Insufficient historical replication.
- Validator downtime.
- Byzantine committees.
- Committee capture.
- Randomness manipulation.
- Network partitions.
- Governance deadlock.
- Coordinated governance capture.
- Infrastructure migration.
- Founder infrastructure temporarily unavailable.
- Snapshot synchronization.
- Constrained CPU, memory, disk and bandwidth.
- Arithmetic overflow and rounding boundaries.

The simulator must distinguish economic abstractions from real cryptographic and networking benchmarks.

Synthetic contribution and storage scores are permitted for economic scenarios, but do not establish the security of production verification mechanisms.

## Measurements

Record:

- Outstanding NC.
- Outstanding WC.
- Redemption liabilities.
- Effective monetary supply.
- Gross issuance.
- Net expansion.
- Burning.
- Reward distribution.
- Staking participation.
- Validator concentration.
- Committee threshold risk.
- Developer compensation.
- Storage availability.
- Governance participation.
- Governance capture risk.
- Synchronization bandwidth.
- Ledger growth.
- Active state growth.
- CPU utilization.
- Memory utilization.
- Arithmetic safety.

All provisional parameters must be independently configurable, including MVB and committee settings, withdrawal and penalty parameters, governance thresholds and periods, replication and snapshot assumptions, in addition to monetary coefficients.

Do not claim economic sustainability or security guarantees before validation.

## Adversarial and boundary coverage

Retain the earlier repeated partial redemption, artificial replication demand, reward timing, repeated committee sampling, adaptive attacker and correlated infrastructure failure cases. Report actual NC minting separately from effective gross issuance and redemption settlement. Economic simulation may use explicit hypothetical time and scoring assumptions. Real implementations must be benchmarked separately for verification throughput, CPU, memory, disk, native encoding size, synchronization and residential bandwidth.
