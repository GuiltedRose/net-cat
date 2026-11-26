# Genesis

## Genesis monetary state

The genesis mint is exactly 50,000,000 NC.

All 50 million NC exist in the genesis monetary state. Do not reinterpret this amount as a gradually released allocation or future issuance ceiling.

Initial WC supply is zero.

Genesis is the only exceptional initialization mint.

All subsequent issuance is authorized exclusively by deterministic network rules.

Actual genesis recipients, allocation distribution, initial liquidity arrangements and production validator bootstrapping are deferred to launch preparation.

The whitepaper does not require finalized recipient addresses or exchange commitments.

Simulations may use hypothetical genesis distributions.

Publish the mainnet allocation and genesis state for independent verification. Genesis creates no privileged recipient, validator or upgrade authority. Hypothetical simulation and development allocations do not establish production commitments. See [consensus](CONSENSUS.md) and [simulation](SIMULATION.md).

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

Hypothetical balances begin with zero WC and zero unsettled redemption credits. See [simulation methodology](SIMULATION.md#simulation-methodology).
