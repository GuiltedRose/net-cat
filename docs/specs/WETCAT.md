# WetCat (WC)

## Approved requirements

WC is automatically awarded for eligible developer contributions and automatically participates in value appreciation. It requires no staking entry burn. Consensus prohibits account and wallet transfers, direct purchase, exchange trading, deposits and withdrawals, decentralized exchange trading and bridging. There is no general-purpose WC transfer transaction. Partial and full redemption permanently destroy all WC selected and convert its redemption value into NC.

The approved initial conversion ratio is **5 WC : 1 NC**. The initial per-account accumulation limit is 1% of the genesis NC supply in NC redemption value. At genesis, `50,000,000 NC × 1% = 500,000 NC`, and `500,000 NC × 5 WC/NC = 2,500,000 WC`. The WC quantity is a derived starting value. The cap adjusts dynamically and applies to the base developer identity across wallet replacement, multiple associated wallets and wallet transfers. The daily cap model and treatment of balances above the cap are specified below.

WC uses three decimal places, 1,000 atomic units per WC, `u64` balances and `u128` aggregate accounting. WC has no fixed global supply cap. Consensus enforces these restrictions directly, including no exchange listings.

## Shared issuance and redemption

Outstanding WC liability is `L_WC = W × R`. Total monetary liabilities also include unpaid redemption credits. The [effective supply identity](MONETARY_PROTOCOL.md#effective-monetary-supply) counts WC, credits and minted NC exactly once. Redemption is one-way and has no staking withdrawal delay.

## WC appreciation eligibility

Every outstanding WC unit participates in global appreciation, including balances above the current accumulation cap.

```text
B_WC = 0.15 × E_day

R_next = R_current + B_WC / W
```

Where W is total outstanding WC at the defined pre-award settlement snapshot.

If W is zero, leave the index unchanged and the corresponding allocation unissued.

Apply appreciation before new WC awards.

Newly awarded WC cannot receive appreciation from a period preceding its existence.

The actual increase in redemption liabilities must not exceed the appreciation allocation.

Round index increases conservatively.

## Fixed-point representation

Retain:

| Monetary field | Representation |
|---|---|
| NC balances | u64, 5 decimal places |
| WC balances | u64, 3 decimal places |
| Aggregate accounting | u128 |
| Internal WC conversion index | Provisional 12 fractional decimal digits |

The conversion index represents NC per WC.

Specify its integer width and intermediate arithmetic requirements before simulator implementation.

Consensus-critical calculations must use deterministic integer arithmetic.

Do not use floating-point arithmetic for authoritative monetary state transitions.

Wide intermediates must be checked. Where u128 is insufficient, explicitly specify a safe wider-arithmetic strategy.

## Redemption remainders

Preserve strictly one-way WC-to-NC redemption.

When redemption produces a fractional NC atomic unit:

- Retain the remainder as an account-bound redemption credit.
- Combine it with future redemption credits.
- Mint NC only when the accumulated amount is representable.
- Include unsettled credits in monetary liability accounting.
- Do not round redemptions upward.
- Do not silently destroy outstanding monetary liabilities.

Extend the effective supply accounting identity as necessary to include unsettled redemption credits without double-counting WC or minted NC.

Repeated partial redemptions must not generate additional value through rounding.

Wallet replacement must not reset outstanding redemption credits.

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

Recalculate the cap daily using effective supply after appreciation and before new contribution awards. Round awardable quantities down to WC atomic units.

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

Adaptive p is a future simulation extension. Its target function remains open.

WC has no consensus or governance voting power. Redemption is strictly one-way from WC to NC. NC cannot be converted back into WC. See [contribution award conversion](REWARDS.md#contribution-valuation).

## Open decisions

Define the exact finalized settlement cutoff and ordering for redemptions relative to appreciation, credit and index widths and intermediate arithmetic, and conservative quantization and reconciliation. The 12-place index multiplied by three-place WC quantities can produce NC values with 15 fractional digits before any minting conversion. Credits do not constitute outstanding WC and receive no WC index appreciation. See [daily settlement](MONETARY_PROTOCOL.md#deterministic-daily-settlement).
