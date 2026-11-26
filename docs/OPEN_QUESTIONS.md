# Open questions

[Whitepaper v0.5.1](../WHITEPAPER.md) and [D-040 through D-046](DECISIONS.md#v051-decisions) establish the provisional simulation baseline.

## Simulator implementation prerequisites

- Specify fixed-point integer widths, intermediate capacity, rounding directions, daily and annual reconciliation, and credit quantization. A 12-place NC-per-WC index multiplied by WC with three decimal places can yield 15 fractional NC digits. Preserve those liabilities through repeated partial redemptions.
- Define exact finalized transaction cutoffs, year rollover and day assignment, annual and daily division remainders, which utilization state carries forward, and whether the day’s utilization is staged at the previous cutoff. Measure pressure over matched finalized periods and define which authorized gross envelope is its denominator. Prevent same-period issuance from feeding its own budget.
- Specify incomplete 30-day window startup, target averaging, observable account attribution, zero-supply behavior for the burn ratio, and event ordering for redemption, appreciation, award caps and unbonding.
- Specify synthetic participation and scoring, fee assumptions, attainable staking and bonding, admission and rejection policies, MVB population and grace-period modeling, and participant behavior during congestion, downtime and governance votes. Desired populations and workload are inputs, and eligibility and finalization are outputs.
- Specify adoption and workload interpolation, post-year-one growth, transaction mix, burst and expiry behavior, deterministic generator, seeds and sampling order. Define efficient aggregation and how to reconcile aggregated events with exact daily and annual accounting.
- Set quantitative economic acceptance thresholds before claiming validation. Test both governance approval denominators with default participating power, record checkpoint outputs and analytical limitations, and compare results across all nine scenarios. No production allocations or market commitments are needed.

## Production implementation

- Finalize fixed-point scales, integer widths and rounding, annual budget enforcement, redemption neutrality, zero-denominator behavior, and slashing and reward ordering. Specify treatment of locked principal changes, compounding, and eligibility while unbonding.
- Specify locking, rounds, proposers, timeouts, epoch handover, verified participation and inactive status, penalty identity and evidence ordering, exact severe offenses, and deterministic withdrawal and evidence windows. Analyze safety and liveness under concentration, partitions, long-range attacks and repeated seats. Detail secure randomness generation, seed evolution, ECVRF ciphersuite and unbiased sampling.
- Specify signing domains, nonce rules, canonical framing and encoding, state tree namespace order and empty roots, checkpoint trust, authenticated snapshot manifests, resource bounds and retention minima. Define the finality certificate format and whether the proposer index refers to a seat or validator. Keep history availability distinct from state reconstruction.
- Define peer diversity and bootstrap trust, message and traffic budgets, payload boundary accounting, and validation under interrupted residential connections. Design archival evidence that can support pruning decisions despite unverifiable future availability or shared ownership.
- Specify decentralized contribution evidence, impact assessment, attribution and disputes, verifiable storage and anti-farming mechanisms, git migration verification, registry funding, and authorization freshness. Define private metadata and transfer proofs, key distribution, AEAD nonce handling, rotation and revocation.
- Define identity recovery thresholds and conflict handling, pending earnings and wallet eligibility, role-separated keys, addresses and hardware signing, browser boundaries and chain-specific recovery.
- Specify proposal admission, major amendment classification, production approval denominator, abstention encoding, snapshot and collateral locking, exact review and activation timing, upgrade compatibility and executable activation, amendment procedures, and emergency handling without unilateral authority.
- Specify exact fees, scheduling, spam resistance, the standard transaction class and USD measurement basis. Design exchange interfaces with verifiable finality and reconciliation while preserving chain independence.

## Benchmarking and security review

- Measure hashing, Ed25519 and candidate BLS12-381 storage and verification costs, device compatibility, proof size, Merkle update costs, serialization and Zstandard compression. If aggregation is adopted, specify and analyze aggregation security.
- Measure network and validator loads at the proposed committee sizes and block limit. Benchmark QUIC and fallback, NAT traversal and relays, sync under constrained CPU, memory, disk and bandwidth, snapshot verification and interruption recovery, ledger growth and migration.
- Validate simulation baselines for issuance, stake concentration, governance capture, WC liabilities, replica thresholds and resource targets before production adoption. Plan independent audits and reproducible signed releases.

## Launch preparation

- Choose actual recipients and allocations for the approved genesis mint, publish independently verifiable genesis artifacts, and establish production validator distribution and independent bootstrap infrastructure.
- Establish initial liquidity arrangements, utility criteria, and any exchange applications or commitments. Obtain regulatory review. Optional wallet conversion and a native DEX remain separate expansions.
- Obtain professional legal review and documented rights-holder authorization before any explicit NCOSL adoption. BSD-3-Clause remains active and hosted repository owners choose their own licenses.

## Implemented research conventions

The [research simulator guide](SIMULATOR.md) records the experimental choices used to run v0.5.1, including atomic units and wide arithmetic, daily and annual cutoffs, startup observation windows, seeded sampling, synthetic workload, and cohort modeling. These choices support reproducible research without resolving production consensus-time, proofs, identity and governance requirements. Empirical comparison of cohort and explicit-account outcomes, throughput measurement, long-horizon resource profiling and economic acceptance remain open.
