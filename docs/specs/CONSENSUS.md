# Consensus

## Approved architecture

NullCat uses permissionless Proof of Stake and rotating validator committees with Tendermint-style Byzantine Fault Tolerant finality. Committee selection is cryptographically randomized and stake-weighted. Finalization requires **greater than two-thirds** of committee voting power. Nodes independently verify transactions and state transitions. Passive staking is separate from active validation. Active validators maintain separate slashable bonded NC, with no founder-controlled or privileged class.

Occasional downtime costs rewards. Prolonged inactivity may remove eligibility, and provable malicious consensus behavior may result in slashing. Validator withdrawals require a suitable unbonding period. The combined design requires its own safety and liveness analysis covering Byzantine fault assumptions, locking, round changes, committee transitions, randomness manipulation, partitions and long-range attacks. Committee sampling and Tendermint-style voting alone do not establish those guarantees.

Validators must support pruned operation. The security model must specify assumptions about Byzantine voting power in sampled committees, network timing and partition recovery, and how independently verified finalized state survives committee transitions and long-range attacks. The system requires analysis of selection bias, concentration and randomness manipulation.

The following formulas and values are provisional simulation baselines. Parameters, resource requirements and combined safety and liveness require validation.

## Minimum Validator Bond

Define the Minimum Validator Bond (MVB):

```text
B_min = S × β
```

Initial provisional coefficient:

```text
β = 0.00001
```

This is 0.001% of outstanding NC.

At genesis:

```text
B_min = 500 NC
```

The MVB is recoverable collateral, not the amount burned.

The bond requirement applies after any applicable entry burn.

Provisional adaptive model:

```text
β_target = β_initial × N / N_target

β_next = 0.95 × β_current + 0.05 × β_target
```

| Parameter | Provisional value |
|---|---:|
| Initial coefficient | 0.001% |
| Minimum coefficient | 0.0001% |
| Maximum coefficient | 0.01% |
| Target eligible validator population | 1,000 |
| Adjustment interval | Daily |

Clamp the coefficient to its provisional bounds.

Validator count manipulation and stake concentration must be simulated.

Changes to eligibility requirements should provide a suitable grace period.

## Validator activation

Validator eligibility is automatic upon meeting the MVB.

Actual validator activation remains voluntary.

Participants must explicitly accept:

- Consensus responsibilities.
- Validator signing requirements.
- Collateral locking.
- Slashing exposure.
- Unbonding requirements.

Existing passive stake may become validator collateral without a duplicate entry burn if continuously locked.

The same NC cannot simultaneously receive passive staking and active validator rewards.

## Committee size

```text
K = min(N, max(K_min, min(K_max, ceil(k × sqrt(N)))))
```

Provisional parameters:

```text
K_min = 64
K_max = 256
k = 4
```

N is the eligible active validator population.

K represents sampled voting seats, not necessarily unique validator machines.

The selected committee may contain repeated selections of the same validator.

Committee size remains subject to security and bandwidth analysis.

Do not claim that 64–256 seats provide sufficient security under every stake distribution.

## Committee selection and voting power

Adopt stake-weighted sampling:

```text
p_i = B_i / sum(B_j)
```

Use independent stake-weighted draws with replacement as the initial simulation model.

Each sampled seat carries one unit of voting power.

A validator selected multiple times carries the corresponding number of votes.

Do not apply bonded stake weighting a second time.

```text
sum(v_i) = K

F = floor(2K / 3) + 1
```

F is the finalization threshold.

Sampling must use unpredictable, independently verifiable randomness resistant to manipulation.

Actual randomness generation, seed evolution and committee handover rules remain security requirements.

## Consensus epochs and rotation

Provisional parameters:

| Parameter | Value |
|---|---:|
| Target block interval | 10 seconds |
| Blocks per consensus epoch | 360 |
| Expected epoch duration | Approximately 1 hour |
| Committee rotation | Every epoch |
| Monetary adjustment | Daily |

Calculate the epoch from block height:

```text
epoch = floor(block_height / 360)
```

Block intervals are targets rather than guaranteed wall-clock durations.

The current committee must authorize the finalized state establishing its successor.

Consensus stalls cannot authorize unilateral committee replacement.

Exact Tendermint locking, round changes, proposer selection, timeout behavior and handover rules remain required technical specifications.

## Validator reward mathematics

```text
B_V = 0.30 × E_actual

Q_i = sum(v_i,e × A_i,e)

R_i = B_V × Q_i / sum(Q_j)
```

Where:

- v_i,e = assigned voting seats during epoch e.
- A_i,e = verified participation coefficient within [0, 1].

Do not multiply by bonded stake again after stake-weighted selection.

Occasional downtime results in missed rewards.

Prolonged inactivity may remove validator eligibility.

Participation evidence and settlement periods require precise specification.

## Slashing

```text
L_i = B_i × s

B_i,next = B_i - L_i
```

Provisional penalty parameters:

| Violation | Penalty |
|---|---:|
| Occasional downtime | 0% |
| Prolonged inactivity | Eligibility removal |
| Provable equivocation | 10% |
| Severe provable consensus safety violation | Up to 100% |

Slashed NC is permanently burned.

Penalties require independently verifiable evidence.

Duplicate evidence cannot repeatedly punish the same offense.

Exact severe offenses, evidence format, penalty ordering and correlated slashing remain open.

## Unbonding

Provisional withdrawal delays:

| Category | Delay |
|---|---|
| Passive staking | 1 day |
| Active validator collateral | 30 days |
| WC redemption | No staking withdrawal delay |

Partial withdrawals remain supported.

Validator collateral remains slashable throughout unbonding.

Evidence retention must remain compatible with withdrawal delays.

Define deterministic consensus-time or block-height accounting for these durations. Do not silently equate approximate block intervals with exact wall-clock time.

## Committee security analysis

For independent unbiased sampling with replacement, use the binomial tail model:

```text
P_threshold =
sum from j = ceil(K / 3) to K of:

C(K, j) × a^j × (1-a)^(K-j)
```

Where a represents adversarial bonded-stake share.

This measures the probability of reaching the one-third Byzantine voting threshold within a sampled committee.

It is not the probability of a successful theft or complete network compromise.

Simulate:

- Repeated committee selections.
- Adaptive attackers.
- Concentrated stake.
- Wallet splitting.
- Correlated infrastructure failures.
- Randomness manipulation.
- Network partitions.
- Long-range attacks.

## Open specifications

Specify deterministic timing, validator eligibility grace periods, voting and participation evidence, committee handover proof and state commitments, proposer selection, locking and round changes, randomness and checkpoint trust. The binomial model assumes independent unbiased draws and a fixed adversarial share. It measures threshold exposure for one sampled committee, and repeated seats may concentrate votes in fewer operators and failure domains.
