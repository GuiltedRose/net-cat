# Monetary protocol

## Monetary architecture and representation

NC is freely transferable and native to NullCat. WetCat (WC) is an account-bound, non-transferable reward asset. Neither has a fixed global supply cap. The approved genesis mint is **50,000,000 NC**. Distribution and validator bootstrapping remain open. Mainnet distribution must be public and independently verifiable. Deterministic issuance and burns govern monetary expansion. The design seeks gradual expansion, periods of deceleration and stabilization, and subsequent expansion. All native transaction fees burn permanently. There is no prefunded reward pool, privileged minting authority or automatic founder allocation.

| Parameter | NC | WC |
| --- | --- | --- |
| Balance representation | `u64` | `u64` |
| Decimal precision | 5 | 3 |
| Atomic units per coin | 100,000 | 1,000 |
| Balance storage | 8 bytes | 8 bytes |
| Aggregate accounting | `u128` | `u128` |

Consensus-critical calculations use deterministic fixed-point integer arithmetic with checked wide intermediates. Any operation whose intermediates exceed `u128` needs an explicitly specified safe arithmetic strategy. Reject overflowing transitions and prohibit integer wrapping and silent truncation. Representation limits are technical safety boundaries. Decimal constants in the simulation equations express mathematical values for integer implementation.

### Effective monetary supply

| Symbol | Meaning |
| --- | --- |
| `S` | Outstanding NC, including locked and staked NC |
| `W` | Outstanding WC |
| `R` | NC redemption value per WC |
| `L` | Outstanding WC redemption liabilities in NC |
| `T` | Effective monetary supply in NC |

```text
L = W × R
T = S + L
```

WC awards and appreciation create liabilities charged to the shared issuance budget. Redemption settles an existing liability. It moves an equivalent value from `L` into `S` and must create zero additional effective issuance. Settlement must reconcile any rounding differences and prevent double-counting NC already minted and held for payment and the unminted portion of monetary obligations. The appreciation index, scale conversions, transaction ordering, rounding and liability reconciliation remain unresolved.

Gross issuance measures newly issued NC rewards plus the NC value of newly created WC liabilities from awards and appreciation. Effective net expansion is gross issuance less permanent NC burns over the same period, after reconciling settlement rounding. The model’s `E_actual` determines the utilized gross budget before any unused allocations. Actual NC minting includes NC rewards and NC created to settle WC redemptions. That settlement offsets the mint with a liability reduction and consumes no new issuance budget. Locks leave outstanding NC unchanged.

## Provisional simulation models

The equations and coefficients below follow [whitepaper v0.4.0](../../WHITEPAPER.md). Deterministic period rules and arithmetic scales require approval before production.

### Annual gross issuance ceiling

```text
G = 50,000,000 NC
r(T) = 0.01 + 0.04 × G / max(G, T)
E_max(T) = T × r(T)

For T ≥ G:
E_max(T) = 0.01 × T + 2,000,000 NC
```

`G` is the genesis mint, `r(T)` is the maximum annual gross issuance rate and `E_max(T)` is the annual ceiling. The rate starts at 5% when `T = G`, remains 5% below `G` and approaches 1% as `T` grows. The ceiling limits gross issuance. It creates no obligation to issue. Deterministic reward-period budgets must avoid exceeding the annual intent through compounding, repeated recalculation or rounding. The reference supply, period accounting and budget reset rules remain open.

### Dynamic utilization

```text
E_actual = E_max × U
0 ≤ U ≤ 1
U_next = 0.95 × U_current + 0.05 × U_target
U_target = 0.40 × V + 0.40 × D + 0.20 × M

Q = S × q
V = min(1, A / Q)
```

`U` is issuance utilization, updated daily in the provisional model. The normalized inputs `V` (validator participation), `D` (qualified developer activity) and `M` (monetary stability) each lie in [0, 1]. `A` is eligible actively participating bonded NC and `Q` is target active bonded NC, with provisional `q = 10%`. Monetary stability must also constrain issuance independently of the weighted utilization target, so participation cannot override monetary safety limits. Raw transaction volume, commit counts and account creation cannot independently justify issuance. Daily timing, startup utilization, normalization, manipulation resistance, handling `Q = 0`, the independent safety constraint and deterministic rounding remain open.

### Shared reward budget

| Category | Provisional allocation |
| --- | ---: |
| Active validator rewards | 30% |
| WC contribution rewards | 30% |
| Passive NC staking rewards | 15% |
| WC value appreciation | 15% |
| Storage rewards | 10% |
| Total | 100% |

All monetary rewards draw from this budget, including the NC value of new WC liabilities. Active contributions and infrastructure should collectively receive more compensation than passive ownership. Unused allocations remain unissued and do not automatically move to other categories. No category may independently exceed the shared issuance budget. Validator reward scoring remains open.

### Progressive NC staking entry burn

Passive NC staking requires a permanent entry burn. A network baseline of **0.01%–2%** is separate from the participant concentration penalty. In this equation `M`, `P` and `C` are normalized monetary pressure, staking saturation and network concentration pressure. Monetary pressure here is distinct from monetary stability in the utilization equation.

```text
b_target = 0.0001 + 0.0199 × (0.5M + 0.3P + 0.2C)
b_network_next = 0.95 × b_network_current + 0.05 × b_target

h = H / S
b_individual = b_network + 0.30 × sqrt(h)
burned_amount = deposit × b_individual
staked_principal = deposit - burned_amount
```

The network baseline is smoothed daily. `H` is the participant's eligible NC holdings, including staked holdings. The provisional concentration coefficient is 0.30. Use a deterministic integer square root with specified fixed-point scaling. Individual rates can exceed 2% and must remain mathematically valid and below 100%. Apply the rate only to new deposits. Partial and full unstaking incur no new entry burn, and later deposits use the rate then applicable.

The definitions of the baseline inputs, initial baseline, observation timing, zero-supply handling and burn rounding remain open. Unrelated wallets cannot necessarily be attributed to one beneficial owner. Simulate wallet splitting and concentration attacks even where accounts can aggregate linked wallets.

### Passive NC staking rewards

```text
P_reward = 0.15 × E_actual
R_i = P_reward × P_i / P_total
```

`P_i` is participant `i`'s time-weighted eligible staked principal and `P_total` is the corresponding total. Time weighting limits reward-period deposit manipulation. With zero eligible participation, the allocation remains unissued. Percentage yield is an outcome of distribution. There is no independent fixed APY or separate issuance mechanism for yield. Settlement, compounding, rounding, minimum participation and withdrawal conditions remain open where unspecified.

See [fees](FEES.md), [WetCat](WETCAT.md), [rewards](REWARDS.md) and [simulation](SIMULATION.md).
