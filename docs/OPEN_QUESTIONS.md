# Open questions

[Whitepaper v0.5.0](../WHITEPAPER.md) and [D-029 through D-039](DECISIONS.md#v050-decisions) establish the current baseline. The following gates describe the decisions still needed.

## Integrated simulation

- Specify the annual budget reference and period boundaries. Define how daily utilization, safety constraints, burns, reward settlement and unused allocations affect spending without repeated budget creation. Define simulated calendar or height semantics, rounding and reconciliation, including tiny values and zero denominators.
- Define initial utilization and burn baseline, normalization targets and windows, monetary pressure and stability metrics, and the separate stability constraint. Synthetic scores and hypothetical genesis allocations are permitted with assumptions recorded. Compare feedback behavior and sensitivity and define acceptance criteria before drawing economic conclusions.
- Define WC eligibility for index appreciation, snapshot and settlement order, rounding, and cap-state reference timing. A global rate increases the liability of every outstanding unit valued at that rate. If eligible units differ from all outstanding units, specify how the global index and `L = W × R` can account for that difference without overspending. Define future adaptive cap target separately.
- Define the validator population counted for MVB adaptation and distinguish it from the active committee sampling population. Model eligibility feedback, grace periods, count manipulation, correlated seat loss, the possibility of empty committees, and repeated threshold exposure. The binomial estimate is conditional on its fixed-share and independent draw assumptions.
- Specify economic assumptions for fees, staking time weighting, withdrawal clocks, participation scores, archival availability and governance participation. Test governance quorum under both possible approval denominators, without treating either as adopted. Actual genesis recipients and exchange commitments are unnecessary for simulation.

## Production implementation

- Finalize fixed-point scales, integer widths and rounding, annual budget enforcement, redemption neutrality, zero-denominator behavior, and slashing and reward ordering. Specify treatment of locked principal changes, compounding, and eligibility while unbonding.
- Specify locking, rounds, proposers, timeouts, epoch handover, verified participation and inactive status, penalty identity and evidence ordering, exact severe offenses, and deterministic withdrawal and evidence windows. Analyze safety and liveness under concentration, partitions, long-range attacks and repeated seats. Detail secure randomness generation, seed evolution, ECVRF ciphersuite and unbiased sampling.
- Specify signing domains, nonce rules, canonical framing and encoding, state tree namespace order and empty roots, checkpoint trust, authenticated snapshot manifests, resource bounds and retention minima. Define the finality certificate format and whether the proposer index refers to a seat or validator. Keep history availability distinct from state reconstruction.
- Define peer diversity and bootstrap trust, message and traffic budgets, payload boundary accounting, and validation under interrupted residential connections. Design archival evidence that can support pruning decisions despite unverifiable future availability or shared ownership.
- Specify decentralized contribution evidence, impact assessment, attribution and disputes, verifiable storage and anti-farming mechanisms, git migration verification, registry funding, and authorization freshness. Define private metadata and transfer proofs, key distribution, AEAD nonce handling, rotation and revocation.
- Define identity recovery thresholds and conflict handling, pending earnings and wallet eligibility, role-separated keys, addresses and hardware signing, browser boundaries and chain-specific recovery.
- Specify proposal admission, major amendment classification, quorum denominator, abstentions, snapshot and collateral locking, exact review and activation timing, upgrade compatibility and executable activation, amendment procedures, and emergency handling without unilateral authority.
- Specify exact fees, scheduling, spam resistance, the standard transaction class and USD measurement basis. Design exchange interfaces with verifiable finality and reconciliation while preserving chain independence.

## Benchmarking and security review

- Measure hashing, Ed25519 and candidate BLS12-381 storage and verification costs, device compatibility, proof size, Merkle update costs, serialization and Zstandard compression. If aggregation is adopted, specify and analyze aggregation security.
- Measure network and validator loads at the proposed committee sizes and block limit. Benchmark QUIC and fallback, NAT traversal and relays, sync under constrained CPU, memory, disk and bandwidth, snapshot verification and interruption recovery, ledger growth and migration.
- Validate simulation baselines for issuance, stake concentration, governance capture, WC liabilities, replica thresholds and resource targets before production adoption. Plan independent audits and reproducible signed releases.

## Launch preparation

- Choose actual recipients and allocations for the approved genesis mint, publish independently verifiable genesis artifacts, and establish production validator distribution and independent bootstrap infrastructure.
- Establish initial liquidity arrangements, utility criteria, and any exchange applications or commitments. Obtain regulatory review. Optional wallet conversion and a native DEX remain separate expansions.
- Obtain professional legal review and documented rights-holder authorization before any explicit NCOSL adoption. BSD-3-Clause remains active and hosted repository owners choose their own licenses.
