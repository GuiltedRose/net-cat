# Integrated simulation requirements

Use deterministic fixed-point monetary arithmetic, checked intermediates and reproducible runs. Record inputs, algorithm and seed configuration. The [monetary protocol](MONETARY_PROTOCOL.md), [consensus](CONSENSUS.md) and [governance](GOVERNANCE.md) define the provisional baseline. Hypothetical genesis allocations and synthetic contribution and storage scores support economic modeling. Separate real cryptography, networking and resource benchmarks from simulated or analytical estimates.

## Primary adoption matrix

Establish exactly nine primary baseline scenarios divided into three congestion categories.

All scenarios begin with the same 50 million NC genesis mint and use identical baseline monetary and consensus parameters.

| Scenario | Users after 30 days | Users after 1 year | Active developers | Validators | Storage operators |
|---|---:|---:|---:|---:|---:|
| L1 — Near dormant | 10 | 50 | 5 | 4 | 3 |
| L2 — Minimal | 100 | 500 | 50 | 10 | 5 |
| L3 — Slow adoption | 500 | 5,000 | 500 | 50 | 25 |
| M1 — Modest | 2,500 | 25,000 | 2,500 | 150 | 75 |
| M2 — Expected baseline | 10,000 | 100,000 | 10,000 | 500 | 250 |
| M3 — Strong | 50,000 | 500,000 | 50,000 | 2,000 | 1,000 |
| H1 — Rapid | 100,000 | 1,000,000 | 100,000 | 5,000 | 5,000 |
| H2 — Extreme | 1,000,000 | 10,000,000 | 1,000,000 | 10,000 | 25,000 |
| H3 — Global surge | 10,000,000 | 100,000,000 | 10,000,000 | 50,000 | 250,000 |

Developer, validator and storage populations represent synthetic year-one targets.

These are test assumptions, not adoption forecasts.

Do not assume all requested validator participants can immediately satisfy bonding eligibility. Derive actual eligible participation from monetary state, available NC, applicable burns and the MVB.

## Offered network workload

Use the following provisional workload targets:

| Scenario | On-chain requests/sec | Passive NC staked | Validator bonds | Repository requests/sec |
|---|---:|---:|---:|---:|
| L1 | 0.01 | 1% | 0.1% | 0.1 |
| L2 | 0.1 | 5% | 1% | 1 |
| L3 | 1 | 10% | 3% | 10 |
| M1 | 10 | 15% | 5% | 100 |
| M2 | 50 | 20% | 10% | 500 |
| M3 | 100 | 25% | 15% | 2,500 |
| H1 | 250 | 30% | 20% | 10,000 |
| H2 | 1,000 | 40% | 25% | 50,000 |
| H3 | 10,000 | 50% | 30% | 500,000 |

Passive staking and validator collateral represent distinct portions of outstanding NC.

These percentages are desired scenario conditions, not instructions to create additional NC or bypass eligibility.

Record target participation separately from achieved participation.

Distinguish offered transaction demand from admitted, processed, rejected, expired and finalized transactions.

Registered user count must not directly determine transaction volume.

Define configurable workload interpolation, transaction-type distributions, burst behavior and post-year-one growth assumptions.

## Preliminary capacity model

Preserve:

```text
maximum block payload = 256 KiB

target block interval = 10 seconds

preliminary NC transfer record = 153 bytes
```

This produces a theoretical upper bound of approximately 171 basic transfers per second before additional overhead.

Do not treat this figure as benchmarked throughput.

Account for transaction mix, framing, finality certificates, consensus delays and validation costs.

The high-congestion scenarios deliberately exceed this preliminary capacity.

Congestion testing must measure:

- Transaction backlog.
- Confirmation latency.
- Admission and rejection rates.
- Bounded mempool memory.
- Expired transactions.
- Consensus liveness.
- Recovery after traffic subsides.
- Transaction fairness.
- Resource consumption.

Do not allow unlimited queues or assume all offered transactions must eventually finalize.

Congestion must not introduce dynamic fee increases or priority fee bidding.

## Scenario durations

Evaluate every primary scenario at the following checkpoints:

- 30 days.
- 1 year.
- 5 years.
- 10 years.
- 50 years.
- 100 years.
- 1,000 years.

This establishes 63 primary scenario-duration combinations.

Collect checkpoints during longer runs rather than unnecessarily restarting identical scenarios.

Long-horizon simulations must use efficient aggregated economic events instead of processing every hypothetical network packet or block.

Clearly document approximation boundaries.

Do not treat thousand-year economic simulations as literal network execution benchmarks.

## Additional stress overlays

Preserve the nine primary scenarios as the core baseline matrix.

Apply additional stress overlays independently:

- Adoption surge followed by collapse.
- Prolonged inactivity followed by rapid growth.
- Sustained extreme adoption.
- Whale-dominated distribution.
- Predominantly developer adoption.
- Predominantly investor adoption.
- Predominantly storage participation.
- Contribution farming.
- Storage farming.
- Wallet splitting and Sybil participation.
- Mass passive unstaking.
- Mass validator withdrawal.
- Simultaneous WC redemption.
- Rapid WC liability accumulation.
- Insufficient archival replication.
- Founder infrastructure migration.
- Correlated validator downtime.
- Network partitions.
- Governance capture and deadlock.
- Arithmetic representation boundaries.

These overlays supplement rather than replace the nine primary scenarios.

## Parameter sensitivity

Use the current approved baseline for the first simulation pass.

Subsequent sensitivity testing should include:

| Parameter | Conservative | Baseline | Aggressive |
|---|---:|---:|---:|
| Initial annual issuance ceiling | 3% | 5% | 8% |
| Asymptotic annual issuance ceiling | 0.5% | 1% | 2% |
| Daily smoothing | 1% | 5% | 10% |
| Holdings concentration burn coefficient | 15% | 30% | 45% |
| WC accumulation coefficient | 0.5% | 1% | 2% |

Also support configurable:

- Reward allocation percentages.
- Utilization weights.
- Normalization targets.
- Monetary stability constraints.
- MVB coefficients.
- Committee sizes.
- Rotation intervals.
- Slashing percentages.
- Unbonding periods.
- Governance thresholds.
- Storage scoring coefficients.
- Replication thresholds.
- Initial ownership concentration.

Maintain reward allocation totals at 100%.

Changing a simulation parameter must not automatically modify the approved protocol specification.

## Simulation methodology

Use three testing stages:

### Stage 1 — Baseline

Run all nine scenarios with the current provisional protocol parameters.

Use multiple deterministic random seeds.

Record all configuration values and assumptions.

### Stage 2 — Sensitivity

Vary individual parameters to identify their economic and security effects.

Distinguish causation from correlated scenario changes.

### Stage 3 — Combined parameter evaluation

Test promising combinations of parameters across all nine primary scenarios.

Do not optimize exclusively for the expected middle scenario.

Prefer configurations that remain acceptably stable across low, moderate and high adoption.

Do not claim a universally optimal configuration without sufficient evidence.

## Hypothetical genesis distributions

The simulator must support multiple hypothetical distributions of the same 50 million NC genesis mint:

- Broadly distributed holdings.
- Concentrated holdings.
- Extreme whale ownership.
- Low initial staking participation.
- High initial staking participation.
- Small initial validator population.

All hypothetical genesis balances must sum to exactly 50 million NC.

No actual production recipients, public sale, exchange commitment or liquidity provider allocation is required.

## Required simulation measurements

Record at minimum:

### Monetary

- Outstanding NC.
- Outstanding WC.
- WC redemption liabilities.
- Unsettled redemption credits.
- Effective monetary supply.
- Annual authorized issuance.
- Actual effective issuance.
- Actual NC minted.
- Permanent NC burns.
- Net monetary growth.
- Unused issuance allocations.
- WC redemption index.
- Dynamic WC accumulation cap.

### Participation and rewards

- Developer reward distribution.
- Storage reward distribution.
- Validator rewards.
- Passive staking rewards.
- Observed staking yields.
- Staking participation.
- Validator eligibility.
- Entry burn costs.
- New participant accessibility.
- Wealth concentration.
- Reward concentration.

### Consensus and governance

- Committee size and unique validator count.
- Adversarial committee threshold probability.
- Repeated committee exposure.
- Consensus stalls.
- Finalization delays.
- Validator penalties.
- Governance turnout.
- Governance capture.
- Governance deadlock.

### Storage and networking

- Historical ledger growth.
- Active state growth.
- Storage replication.
- Historical data availability.
- Snapshot size.
- Synchronization bandwidth.
- Transaction demand and throughput.
- Backlog size.
- Confirmation latency.
- Estimated resource consumption.

Distinguish analytical estimates from measured engineering benchmarks.

## Hard simulation invariants

The following failures are unacceptable:

- Unauthorized monetary issuance.
- Issuance beyond the shared budget.
- Unaccounted WC redemption liabilities.
- Arithmetic wrapping or silent truncation.
- Invalid finalized state transitions.
- Duplicate monetary settlement.
- Double-counted staking or governance collateral.
- Unauthorized governance amendments.
- Congestion-based fee increases.
- Unauthorized changes to repository permissions.

The simulator must detect and report violations.

Explicitly distinguish rejected invalid transitions from unexpected arithmetic failures or successful invariant violations.

## Economic evaluation criteria

Evaluate:

- Monetary stability.
- Reward sustainability.
- Accessibility for new participants.
- Developer compensation.
- Validator economic incentives.
- Wealth concentration.
- Governance concentration.
- Storage availability.
- Long-term monetary expansion.
- Response to high and low adoption.
- Recovery from economic and network shocks.

Do not invent fiat prices or guaranteed purchasing power.

Report native-currency economic outcomes separately from hypothetical market-price assumptions.

Quantitative economic acceptance thresholds remain adjustable and must be defined before claiming that the monetary model has passed validation.

## Additional validation coverage

Preserve repeated partial redemptions, reward timing manipulation, artificial replication demand, adaptive committee attackers, biased randomness, temporary founder infrastructure loss, snapshot sync, and constrained CPU, memory, disk and bandwidth. Stress high and low burn regimes and low validator participation; record storage availability, snapshot sizes, CPU, memory and arithmetic safety separately. Long-horizon aggregation must preserve daily settlement, annual budget snapshots, liability creation and destruction, and comparable checkpoint metrics.

All provisional parameters are independently configurable. When varying the initial and asymptotic ceiling rates, vary the curve parameters coherently: the provisional genesis uplift is the initial rate minus the asymptotic rate. Define exact interpolation, synthetic behavior, post-year-one growth, event aggregation, seeded generator and acceptance thresholds before implementation. See [open questions](../OPEN_QUESTIONS.md).
