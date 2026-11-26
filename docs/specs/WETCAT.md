# WetCat (WC)

## Approved requirements

WC is automatically awarded for eligible developer contributions and automatically participates in value appreciation. It requires no staking entry burn. Consensus prohibits account and wallet transfers, direct purchase, exchange trading, deposits and withdrawals, decentralized exchange trading and bridging. There is no general-purpose WC transfer transaction. Partial and full redemption permanently destroy all WC selected and convert its redemption value into NC.

The approved initial conversion ratio is **5 WC : 1 NC**. The initial per-account accumulation limit is 1% of the genesis NC supply in NC redemption value. At genesis, `50,000,000 NC × 1% = 500,000 NC`, and `500,000 NC × 5 WC/NC = 2,500,000 WC`. The WC quantity is a derived starting value. The cap adjusts dynamically and applies to the base developer identity across wallet replacement, multiple associated wallets and wallet transfers. The daily cap model and treatment of balances above the cap are specified below.

WC uses three decimal places, 1,000 atomic units per WC, `u64` balances and `u128` aggregate accounting. WC has no fixed global supply cap. Consensus enforces these restrictions directly, including no exchange listings.

## Shared issuance and redemption

| Symbol | Meaning |
| --- | --- |
| `S` | Outstanding NC, including locked, staked and bonded NC |
| `W` | Outstanding WC |
| `R` | NC redemption value per WC |
| `L` | Outstanding WC redemption liabilities in NC |
| `T` | Effective monetary supply in NC |

```text
L = W × R
T = S + L
```

WC awards and appreciation create liabilities charged to the shared issuance budget. Redemption settles an existing liability. It moves an equivalent value from `L` into `S` and must create zero additional effective issuance. Settlement must reconcile any rounding differences and prevent double-counting NC already minted and held for payment and the unminted portion of monetary obligations. Index eligibility, scale conversions, transaction ordering, rounding and liability reconciliation remain unresolved.

## WC appreciation index

```text
B_WC = 0.15 × E_actual

R_next = R_current + B_WC / W
```

Where W represents eligible outstanding WC before new awards for the reward period.

Every eligible WC unit receives the same proportional appreciation.

Update the global redemption index rather than individually updating every account.

If W is zero, the index remains unchanged and the allocation remains unissued.

Apply appreciation before issuing new WC for the corresponding reward period.

Specify settlement ordering and conservative rounding so newly awarded or immediately redeemed WC cannot capture appreciation improperly.

WC appreciation consumes the shared issuance budget through increased redemption liabilities.

## Dynamic WC accumulation cap

```text
C_WC = T × p / R
```

Where:

- T = effective monetary supply.
- p = accumulation coefficient.
- R = current NC redemption value per WC.
- C_WC = maximum eligible WC accumulation.

The initial p is 1%.

Genesis-derived values:

```text
50,000,000 NC × 0.01 = 500,000 NC

500,000 NC × 5 WC/NC = 2,500,000 WC
```

The 2.5 million WC quantity is derived, not a permanent hardcoded cap.

Recalculate the cap during daily monetary adjustments.

Enforce the cap against the base account identity.

If appreciation or monetary contraction places an account at or above its current cap:

- Do not confiscate WC.
- Do not forcibly redeem WC.
- Existing WC continues appreciating.
- Block further WC awards until sufficient capacity becomes available.
- Partial redemption may restore eligibility.

The initial simulator may retain p = 1% while the actual WC quantity limit remains dynamic.

A future adaptive coefficient can use:

```text
p_next = 0.95 × p_current + 0.05 × p_target
```

The exact target function remains open.

WC has no consensus or governance voting power. Redemption is strictly one-way from WC to NC. NC cannot be converted back into WC. See [contribution award conversion](REWARDS.md#contribution-valuation).

## Open decisions

Specify the eligible WC set and its compatibility with a global index and total liability identity, the pre-award snapshot and redemption ordering, award and redemption rounding and dust reconciliation, and the future adaptive cap target. WC redemption has no staking withdrawal delay.
