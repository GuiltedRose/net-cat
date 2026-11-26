# NullCat research simulator

The independent Rust crate in `simulator/` implements a deterministic economic research model for whitepaper v0.5.1. The same configuration, worker dispatcher, economic engine and result schema serve the native desktop and headless interfaces. The engine has no windowing dependency. Production chain behavior, economic suitability and protocol security remain unverified.

## Build and validate on Linux

Use Rust 1.88 or newer and Cargo with access to the dependency registry on first resolution. The desktop needs an X11 or Wayland session and OpenGL support. Install your distribution's compiler, pkg-config, and development packages for OpenGL/EGL, X11, XKB and Wayland as applicable. The headless build skips desktop dependencies.

From the repository root:

```sh
cd simulator
cargo fmt --all -- --check
cargo check --all-targets
cargo test
cargo clippy --all-targets
cargo build --release
./target/release/nullcat-sim gui --width 1440 --height 1000
```

For a headless host, from `simulator/`:

```sh
cargo build --release --no-default-features
./target/release/nullcat-sim defaults --out research.json
./target/release/nullcat-sim validate --config research.json
./target/release/nullcat-sim run --days 30 --seeds 42 --workers 4 --out smoke-report
```

The smoke command covers all nine scenarios and creates nine day-30 checkpoint rows. A failed run is preserved in its report and causes a nonzero CLI exit. The full research command covers up to 63 checkpoints per seed and is intentionally separate:

```sh
./target/release/nullcat-sim run --config research.json --days 365000 --seeds 42,1337,2026 --workers 4 --out full-report
./target/release/nullcat-sim run --scenarios L1,L2 --days 365 --seeds 42 --out bootstrap-report
./target/release/nullcat-sim export --results bootstrap-report/results.json --out bootstrap-rerender --scale 3
```

An initial resolved `Cargo.lock` can be generated with `cargo generate-lockfile`. Retain it with research artifacts if dependency-level reproducibility is required. The implementation handoff did not execute builds, tests or simulations.

## Desktop workspace and exports

Run the desktop binary with `gui`. Edit parameters in the global JSON editor, apply or save the validated config, then choose **Run all scenarios**. Pause, resume and cancel take effect at daily boundaries. The run captures a copy of its parameters. Use the seed filter for comparable overlays and the checkpoint selector to compare the same horizon across scenarios. A saved report can be opened as the current report or loaded as a dotted comparison overlay. Log plots omit nonpositive observations and show their count on hover. The console export and desktop export generate the same report format.

**Export dashboard** produces two independently readable PNGs with consistent scenario colors, configuration identifier, horizon and modeling limitations. Scaling 1–4 controls export dimensions. The same directory contains `results.json`, `config.json`, `checkpoints.csv`, `time-series.csv`, `summary.md` and `diagnostics.log`. Monetary integers are decimal strings in JSON and exact decimal integers in CSV. Amounts in chart toolkits are display-only floating-point values. The CSV contains the exact economic inputs for reported points and does not reconstruct omitted days. Checkpoints and diagnostics are retained independently of chart sampling.

## Configuration

Generate the full editable schema with `defaults`. Fields omitted from the root or parameter groups use the program defaults. Unknown fields, invalid ranges and duplicate scenario IDs or seeds are rejected.

| Group | Purpose |
| --- | --- |
| `days`, `seeds`, `scenarios`, `workers` | Horizon up to 365,000 days, deterministic seeds, selected cases and independent-job concurrency |
| `initial_users`, `population_ceiling`, `annual_growth_ppm`, `whale_bps`, `cohorts` | Adoption profile and hypothetical genesis ownership |
| `economics` | Issuance ceilings, smoothing, burn parameters, normalization targets, reward shares and WC cap |
| `behavior` | Staking adjustments, score, redemption and exit frequency, validator availability, governance votes, onboarding and storage |
| `network` | Analytical block capacity, payload mix, fixed fee, demand, bursts, FIFO and expiry |
| `consensus` | Dynamic MVB, committee and sampling parameters, unbonding and experimental penalty occurrence |
| `transitions` | Ascending scenario switches with a 1-based `day`, destination `to`, and optional current phase `from` |
| `stresses` | Disjoint inclusive windows with `from_day` and `through_day`, plus optional replacement `behavior` or `network` groups |

Rate fields use billionths except `*_bps` and `*_ppm`. Amounts named `*_atoms` use NC atomic units. Set `whale_bps` near zero for broadly distributed holdings, 2500 for the starting configuration, or higher for concentrated ownership. Set `initial_users` low for small initial populations. Switch dates are selected by the operator. For example:

```json
{
  "days": 730,
  "scenarios": ["L1"],
  "transitions": [
    {"day": 90, "from": "L1", "to": "L2"},
    {"day": 180, "from": "L2", "to": "L3"},
    {"day": 365, "from": "L3", "to": "M2"}
  ]
}
```

For rapid adoption use L1→H2, for contraction use M2→L1, and for recovery use H2→M2. Extended L2 needs no transition. A transition switches offered workload immediately and starts population interpolation from the current population. It preserves economic and queued state, including observation and annual budget records. A temporary stress can set zero validator availability or fewer archive copies. A supplied stress group is a full replacement, with any omitted fields in that group taking program defaults. Start from the generated config when preserving base modifications. Stresses cannot change the fee.

## Arithmetic and settlement

- Balances use `u64` NC atoms (10⁻⁵ NC) and WC atoms (10⁻³ WC). The NC-per-WC index uses 12 fractional places. Multiplying WC atoms by the index gives exact amounts at 10⁻¹⁵ NC. Each NC atom equals 10¹⁰ internal units.
- Aggregates and the index use checked `u128`. Multiply/divide uses a guarded 256-bit path when a product overflows `u128`; a product of two `u128` inputs fits exactly in `U256`. Conversions check the destination width. Rate smoothing floors at billionth resolution. Square roots use integer arithmetic. All payouts and entry burns round down at their declared representation. The accounting budget rounds down before allocation, and any remainder of the annual allowance is distributed one internal unit at a time to the first days of the year. Unused allowances never roll over.
- Days are 24-hour simulation periods. At the day boundary capture year-start effective supply when applicable, release matured positions, onboard by transfers, then apply staking changes, slashing, WC redemptions, fees and transfers in stable cohort order. Compute the shared reward envelope from the prior day's saved smoothed utilization and trailing pressure. Apply appreciation to remaining pre-award WC, compute the cap at that state, and settle category rewards. Verify reconciliation and budgets against the day-start value before committing the staged state. Update the parameters from up to 30 completed days, using actual effective issuance, burns and the authorized **pre-utilization** allowances as the pressure window denominator. The unbounded-by-pressure smoothed utilization carries forward, and the pressure clamp is reapplied to each envelope.
- Fractional redemption credits are base-account liabilities separate from WC. Fractions accumulate until they can mint whole NC atoms. Credits never appreciate and wallet association is immaterial. At genesis outstanding NC equals exactly 50 million, with no WC or credits.
- Block-sized workload is analytical. Fee burn is charged only on simulated finalization and paid by the synthetic liquid NC sponsor, the first genesis account. The sponsor's balance bounds affordability. The baseline 0.001 NC fee is experimental. FIFO batches record aggregate days, expiry and flow conservation. The payload model uses a configurable mean record size, framing reserve and liveness sample. No fees increase with congestion.

## Participant and network model limitations

- The first genesis account is explicit. Other accounts are homogeneous cohorts whose individual balance, WC cap, credit and burn are evaluated per member, then multiplied by multiplicity. Onboarding gifts are existing NC from the first account. If funded groups reach the configured limit, additional members enter an unfunded cohort. Falling activity keeps prior wealth and liabilities in the ledger. The participant view tracks requested active population and funded counts, not a full user-session lifecycle.
- Adoption is linear at day 30 and day 365. Later populations hold steady by default, or adjust by a signed growth rate up to the configured ceiling. Developer and storage work use synthetic target counts bounded by active population, with score and reward estimates shared within cohorts. The simulator does not model independent per-person behavior within a cohort. Shared stochastic redemption and withdrawal can produce correlated activity. The baseline adjustment sizes and probabilities are in the generated JSON. Cohort members are not independent security identities.
- Staking approaches target percentages with side effects from burns and balance limits. Passively locked funds can be moved into validator bonds without another burn. Available balance and minimum bond constrain selection. The day-weighted passive score uses entry and exit balances, excluding principal reclassified into bonds. The MVB adapts to estimated eligible active validators. Validator penalties are synthetic events and locked collateral can remain slashable during exit. The model has no production evidence verifier or admission grace period.
- ChaCha8 consumes a stable stream per scenario and seed. Weighted committee draws sample with replacement, including repeated seats. A configurable number of committees sampled per day approximates epoch availability and rewards. Committee threshold probability uses the configured adversarial stake share and is conditional. The simulator omits production committee handover and proposer or message scheduling. Monetary daily settlement remains a research clock even when modeled BFT finalization is stalled. Empty committees finalize no network workload.
- Governance evaluates synthetic votes on reported stake snapshots with the baseline and stricter denominators. Approval, turnout and opposing-chamber behavior are estimates. The model has no proposal execution, governance collateral locking lifecycle or production upgrade authority.
- Storage scoring uses synthetic useful bytes, availability, verification confidence and replica need. Fewer than three other copies flags insufficient archival replication. History and snapshots are size estimates and the model does not delete history, prove replication or benchmark storage and sync.
- Samples retain checkpoints, the first and last successful days, and effective-supply extrema within chart intervals and overall extrema for each plotted metric. Diagnostics preserve flagged condition changes separately. Whole-process peak RSS is Linux-only and includes concurrent workers. Linux worker CPU time and wall time are separate from estimated vector memory and theoretical production loads. Use external profiling for allocations and bandwidth.
- The initial invariant suite is not a security or economics certification. Economic warning thresholds are experimental. Exact chain time, proofs, randomness, secure checkpointing, private repository authorization, actual fees, governance enforcement and real ownership distributions remain production design work.
