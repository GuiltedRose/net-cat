# Storage participation

## Approved requirements

Verifiable storage is an approved participation mechanism. Operators receive NC directly from the shared issuance budget using the provisional 10% allocation with no NC investment or consensus role required. Both eligible repository data and historical ledger archives qualify in principle. Rewards prioritize useful, retrievable data with insufficient replicas and discourage redundant storage, artificial farming and duplicate claims. Operators may prune without slashing or confiscation and stop receiving rewards for data they no longer retain.

## Provisional scoring and settlement

```text
P_i = D_i × A_i × V_i × N_i
R_i = 0.10 × E_actual × P_i / sum(P_j)
```

For storage operator `i`, `D_i` is eligible verifiably stored data and `A_i`, `V_i` and `N_i` are availability, storage verification and network replication need coefficients. The allocation remains unissued if total eligible score is zero.

## Storage limits and availability

Support configurable storage budgets. Empty allocated capacity earns nothing. Verify useful data retention, retrieval and availability. Repository and ledger pruning are independent, and repository inactivity alone must never trigger network-wide deletion of its final available copy. Private repository verification must preserve encryption and supplier-side authorization.

## Open decisions

Storage proofs, eligibility verification, scoring and normalization, replication targets and final-copy detection, anti-farming mathematics, privacy of verification, and reward rounding. See [repositories](REPOSITORIES.md) and [ledger](LEDGER.md).
