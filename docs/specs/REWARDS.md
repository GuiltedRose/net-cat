# Participation rewards

## Approved requirements

All rewards draw from one deterministic [issuance budget](MONETARY_PROTOCOL.md). Financial participation is optional. Developer accounts without an eligible linked wallet can collaborate and do not accrue monetary participation rewards. Pending earnings remain under account control until payout or redemption. Batch contribution evidence and settlement where practical. Native NC transfers and passive staking remain independent of LegacyBound.

## Provisional allocation

| Category | Provisional allocation |
| --- | ---: |
| Active validator rewards | 30% |
| WC contribution rewards | 30% |
| Passive NC staking rewards | 15% |
| WC value appreciation | 15% |
| Storage rewards | 10% |
| Total | 100% |

All monetary rewards draw from this budget, including the NC value of new WC liabilities. Active contributions and infrastructure should collectively receive more compensation than passive ownership. Unused allocations remain unissued and do not automatically move to other categories. No category may independently exceed the shared issuance budget. Validator scoring follows the [provisional consensus model](CONSENSUS.md#validator-reward-mathematics).

### Passive NC staking rewards

```text
P_reward = 0.15 × E_actual
R_i = P_reward × P_i / P_total
```

`P_i` is participant `i`'s time-weighted eligible staked principal and `P_total` is the corresponding total. Time weighting limits reward-period deposit manipulation. With zero eligible participation, the allocation remains unissued. Percentage yield is an outcome of distribution. There is no independent fixed APY or separate issuance mechanism for yield. Settlement, compounding, rounding, minimum participation and withdrawal conditions remain open where unspecified.

### Developer contribution rewards

WC compensates qualified productive contributions. Imported Git history remains intact and generates no retroactive rewards. Eligible new contributions may earn WC after protocol activation and repository registration. Duplicate contributions cannot earn twice. Git timestamps alone cannot establish eligibility. Raw commit count is invalid as a valuation mechanism, including AI-generated commit volume. Founders receive the same treatment as other contributors. Production evidence, attribution, verification and disputes remain open. Ordinary Git operations require no blockchain transaction fee. Cryptographic CTF challenges remain outside the current scope.

### Storage rewards

Verifiable storage is an approved participation mechanism. Operators receive NC directly from the shared issuance budget using the provisional 10% allocation with no NC investment or consensus role required. Both eligible repository data and historical ledger archives qualify in principle. Rewards prioritize useful, retrievable data with insufficient replicas and discourage redundant storage, artificial farming and duplicate claims. Operators may prune without slashing or confiscation and stop receiving rewards for data they no longer retain.

```text
P_i = D_i × A_i × V_i × N_i
R_i = 0.10 × E_actual × P_i / sum(P_j)
```

For storage operator `i`, `D_i` is eligible verifiably stored data and `A_i`, `V_i` and `N_i` are availability, storage verification and network replication need coefficients. The allocation remains unissued if total eligible score is zero. Data eligibility, exact scoring and normalization, proof mechanisms, anti-farming protections and settlement rounding remain open.

See the [storage specification](STORAGE.md). Contribution evidence verification, attribution, dispute resolution, payout cadence and wallet eligibility transitions remain open.

## Generic distribution

```text
B_j = E_actual × a_j

R_i,j = B_j × Q_i,j / sum(Q_j)
```

Where:

- B_j is the category budget.
- a_j is the category allocation.
- Q_i,j is the participant's eligible score.
- R_i,j is the participant's reward.

If total eligible score is zero, the corresponding allocation remains unissued.

Actual yield is an outcome of reward distribution, not an independently defined consensus input.

Reward percentages remain tunable simulation coefficients.

## Contribution valuation

```text
Q_i = sum(A_c × V_c × I_c)
```

Where:

- A_c = contribution acceptance, either 0 or 1.
- V_c = verification confidence within [0, 1].
- I_c = contribution impact within [0, 1].

Distribute contribution rewards:

```text
R_i = 0.30 × E_actual × Q_i / Q_total
```

Convert the resulting NC-equivalent award into WC subject to the account cap:

```text
available_capacity = max(0, C_WC - W_held)

WC_award = min(R_i / R, available_capacity)
```

Amounts blocked by the accumulation cap remain unissued.

Use synthetic contribution scores for initial economic simulation.

The production verification, decentralized evidence and impact assessment mechanisms remain unresolved.

See [validator scoring](CONSENSUS.md#validator-reward-mathematics), [WC appreciation](WETCAT.md#wc-appreciation-index) and [storage](STORAGE.md).
