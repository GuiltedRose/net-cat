# WetCat (WC)

## Approved requirements

WC is automatically awarded for eligible developer contributions and automatically participates in value appreciation. It requires no staking entry burn. Consensus prohibits account and wallet transfers, direct purchase, exchange trading, deposits and withdrawals, decentralized exchange trading and bridging. There is no general-purpose WC transfer transaction. Partial and full redemption permanently destroy all WC selected and convert its redemption value into NC.

The approved initial conversion ratio is **5 WC : 1 NC**. The initial per-account accumulation limit is 1% of the genesis NC supply in NC redemption value. At genesis, `50,000,000 NC × 1% = 500,000 NC`, and `500,000 NC × 5 WC/NC = 2,500,000 WC`. The WC quantity is a derived starting value. The cap adjusts dynamically and applies to the base developer identity across wallet replacement, multiple associated wallets and wallet transfers. Future cap adjustments and how appreciation affects accounts at or above the limit remain open.

WC uses three decimal places, 1,000 atomic units per WC, `u64` balances and `u128` aggregate accounting. WC has no fixed global supply cap. Consensus enforces these restrictions directly, including no exchange listings.

## Shared issuance and redemption

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

## Open decisions

Appreciation index design and formula, the dynamic account cap formula, contribution valuation, and whether or how to apply the accumulation limit when appreciation or a cap adjustment puts an existing account above the limit. Specify redemption authorization, rounding and dust, eligibility after wallet changes, and liability reconciliation. See the [monetary protocol](MONETARY_PROTOCOL.md) and [rewards](REWARDS.md).
