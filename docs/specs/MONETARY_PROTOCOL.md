# Monetary protocol

## Monetary architecture and representation

NC is freely transferable and native to NullCat. WetCat (WC) is an account-bound, non-transferable reward asset. Neither has a fixed global supply cap. The [genesis state](GENESIS.md) contains exactly **50,000,000 NC** and zero WC. Genesis is the only exceptional initialization mint. Distribution and validator bootstrapping remain open. Mainnet distribution must be public and independently verifiable. Deterministic issuance and burns govern monetary expansion. The design seeks gradual expansion, periods of deceleration and stabilization, and subsequent expansion. All native transaction fees burn permanently. There is no prefunded reward pool, privileged minting authority or automatic founder allocation.

| Parameter | NC | WC |
| --- | --- | --- |
| Balance representation | `u64` | `u64` |
| Decimal precision | 5 | 3 |
| Atomic units per coin | 100,000 | 1,000 |
| Balance storage | 8 bytes | 8 bytes |
| Aggregate accounting | `u128` | `u128` |

Consensus-critical calculations use deterministic fixed-point integer arithmetic with checked wide intermediates. Any operation whose intermediates exceed `u128` needs an explicitly specified safe arithmetic strategy. Reject overflowing transitions and prohibit integer wrapping and silent truncation. Representation limits are technical safety boundaries. Decimal constants in the simulation equations express mathematical values for integer implementation.

## Effective monetary supply

Let `S` be outstanding NC including locked and bonded balances, `W` outstanding WC, `R` NC per WC, and `C_credit` total unsettled redemption credits in NC.

```text
L_WC = W × R
L_total = L_WC + C_credit
T = S + L_total
```

WC awards and index appreciation create new liabilities and consume the shared budget. Redemption transfers value out of `L_WC` into minted NC and any unsettled account credit. Credits leave liabilities when paid. Redemption minting is recorded as actual NC minted but is excluded from new effective issuance. Locks and transfers leave `T` unchanged. Permanent burns reduce it without restoring spent budget.

## Provisional simulation rules

`E_day` is the authorized daily reward envelope after utilization and stability limits. Category formulas inherited from v0.5.0 use this envelope where they previously used `E_actual`. Actual effective issuance counts only amounts settled as new rewards or liabilities. Annual ceiling rates remain 5% at and below genesis effective supply and approach 1% as supply grows. For `T ≥ G`, the baseline annual ceiling is `0.01 × T + 2,000,000 NC`.

## Annual issuance accounting

Retain the approved issuance ceiling:

```text
G = 50,000,000 NC

r(T) = 0.01 + 0.04 × G / max(G, T)

E_max = T × r(T)
```

For simulation, establish a monetary year consisting of 365 daily accounting periods.

Calculate the annual issuance ceiling using effective monetary supply at the beginning of the monetary year.

```text
T_reference = effective supply at start of monetary year

B_year = T_reference × r(T_reference)

B_day = B_year / 365

E_day = B_day × U_final
```

Requirements:

- Daily reward settlement.
- Daily monetary parameter adjustments.
- Annual budget reference remains unchanged throughout its accounting year.
- No unintended daily compounding of the annual issuance ceiling.
- Unused daily allocations remain unissued.
- No rollover of unused reward allocations.
- Burning does not replenish the current annual issuance budget.
- Burning influences monetary measurements and subsequent adjustments.
- Conservative integer rounding must never increase the authorized budget.
- Reconcile annual and daily budgets deterministically.

Simulation days represent deterministic 24-hour periods.

Production consensus-time accounting remains a separate specification requirement.

## Initial monetary parameters

Use these provisional simulation values:

| Parameter | Value |
|---|---:|
| Genesis NC supply | 50,000,000 NC |
| Genesis WC supply | 0 WC |
| Initial NC issuance ceiling | 5% annually |
| Asymptotic issuance ceiling | 1% annually |
| Initial issuance utilization | 50% |
| Initial network baseline burn | 0.01% |
| Daily smoothing coefficient | 5% |
| Observation window | 30 days |
| Initial WC accumulation coefficient | 1% |
| Initial WC conversion ratio | 5 WC : 1 NC |

Preserve the absence of fixed global supply caps for NC and WC.

Genesis remains the sole exceptional initialization mint. All subsequent issuance follows network-authorized monetary rules.

## Economic normalization

Reuse the generic normalization function:

```text
N(x, q) = min(1, x / q)
```

Inputs must be nonnegative.

For simulation, if q is zero, return zero.

Use the following provisional measurement targets:

| Measurement | Target |
|---|---:|
| Active validator collateral | 10% of outstanding NC |
| Total locked staking principal | 30% of outstanding NC |
| Qualified developer activity | 1,000 score units per day |
| Observable holdings concentration | Largest observable account holds 1% of outstanding NC |
| Measurement window | Trailing 30 days |

Developer score units do not represent raw commit counts.

Observable holdings do not necessarily identify all wallets belonging to the same beneficial owner.

Define consistent startup behavior for incomplete observation windows.

Use rolling averages where appropriate so measurements and targets use matching units and time periods.

## Monetary pressure and stability

For the observation window:

```text
M_pressure = N(
    max(0, effective_issuance - permanent_burns),
    permitted_gross_issuance
)

M_stability = 1 - M_pressure
```

Use matching observation periods for issuance, burns and permitted issuance.

Retain the weighted utilization model:

```text
U_target = 0.40 × V
         + 0.40 × D
         + 0.20 × M_stability

U_next = 0.95 × U_current
       + 0.05 × U_target
```

Introduce the provisional independent monetary stability constraint:

```text
U_final = min(U_next, 1 - 0.5 × M_pressure)
```

All normalized coefficients and utilization values must remain within [0, 1].

Simulate positive feedback, oscillation, excessive contraction and manipulation of economic measurements.

Do not claim the smoothing mechanism guarantees economic stability or recurring monetary expansion cycles.

## Progressive staking entry burn

Preserve the existing equations:

```text
b_target = 0.0001
         + 0.0199 × (
             0.5 × M_pressure
           + 0.3 × P_saturation
           + 0.2 × C_concentration
         )

b_network_next = 0.95 × b_network_current
               + 0.05 × b_target

b_individual = b_network
             + 0.30 × sqrt(H / S)

burned_amount = deposit × b_individual

staked_principal = deposit - burned_amount
```

Use the approved normalization targets for the initial simulation.

The 0.01%–2% range applies only to the network baseline burn.

The final individual rate includes the holdings-based penalty.

Preserve the requirements for partial unstaking, continuously locked stake conversion, no duplicate entry burns and wallet-splitting analysis.

`V` normalizes eligible active validator bonds against `Q = S × 0.10`. `D` normalizes qualified developer score per day against 1,000. `P_saturation` normalizes total locked staking principal against 30% of outstanding NC, and `C_concentration` normalizes the largest observable account share against 1%. Use the same time basis on both sides of each ratio. Passive and bonded principal do not overlap. Preserve generic smoothing `X_next = X_current + α × (X_target - X_current)` with provisional `α = 0.05`.

Burns apply only to new passive deposits and new validator bonds. Continuously locked passive stake may become validator collateral without a second burn. Partial and complete withdrawals incur no entry burn. Compute the holdings term with deterministic integer square root and require a valid individual rate below 100%. Simulate wallet splitting since unrelated addresses cannot necessarily be attributed to one owner.

### Passive NC staking rewards

```text
P_reward = 0.15 × E_day
R_i = P_reward × P_i / P_total
```

`P_i` is participant `i`'s time-weighted eligible staked principal and `P_total` is the corresponding total. Time weighting limits reward-period deposit manipulation. With zero eligible participation, the allocation remains unissued. Percentage yield is an outcome of distribution. There is no independent fixed APY or separate issuance mechanism for yield. Settlement, compounding, rounding, minimum participation and exact withdrawal timing remain open. The provisional passive withdrawal delay is one day. Passive staking requires no node or minimum validator bond, conveys no consensus voting authority and is outside validator slashing. See [consensus](CONSENSUS.md).

See [fees](FEES.md), [WetCat](WETCAT.md), [rewards](REWARDS.md) and [simulation](SIMULATION.md).

## Deterministic daily settlement

Adopt this provisional ordering:

1. Capture the finalized settlement state and eligible participation measurements.
2. Calculate remaining annual issuance capacity and the authorized daily budget.
3. Apply current utilization and independent monetary stability limits.
4. Divide the daily budget among the five reward categories.
5. Apply WC appreciation to existing outstanding WC.
6. Calculate the pre-award effective supply and updated WC accumulation cap.
7. Calculate and distribute eligible NC and new WC rewards.
8. Reconcile minted NC, WC liabilities, redemption credits and permanent burns.
9. Verify all monetary and arithmetic invariants.
10. Update adaptive parameters for the next accounting period.

Specify the exact transaction cutoff for each settlement snapshot.

Do not allow duplicate settlement of the same accounting period.

Any failed authoritative state transition must leave monetary state unchanged.

## Monetary invariants

Every accounting period must satisfy:

```text
T_next = T_current + E - B

0 ≤ E ≤ E_budget
```

Effective supply includes outstanding NC, WC redemption liabilities and unsettled redemption credits.

Actual issuance includes newly minted NC rewards and new WC obligations.

WC redemption settles an existing obligation and must not independently increase effective supply.

The simulator must detect:

- Budget overspending.
- Unaccounted monetary creation.
- Unaccounted liabilities.
- Double settlement.
- Arithmetic overflow.
- Integer wrapping.
- Invalid negative balances.
- Duplicate reward claims.
- Unauthorized minting.

All category allocations must remain within the authorized shared budget.

Observation inputs and parameter updates must be indexed to finalized periods to avoid depending on the issuance currently being computed. Specify whether the `U_next` used in the current envelope is staged from the prior cutoff or computed at this cutoff, and whether the smoothed value or the capped authorization carries forward. The settlement order and equations do not fix those timing details. See the [open questions](../OPEN_QUESTIONS.md).
