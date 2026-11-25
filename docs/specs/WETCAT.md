# WetCat (WC)

## Approved requirements

WC is an account-bound monetary asset earned through eligible LegacyBound participation. It automatically accumulates redeemable NC value through protocol-defined mechanisms. There is no independently fixed global WC cap. Consensus enforces a maximum WC balance per developer account across wallet replacement and reassociation.

Consensus prohibits direct purchases, wallet-to-wallet transfers, exchange deposits and withdrawals, decentralized exchange trading and cross-chain bridging. These restrictions belong in transaction validity rules. Smart contracts, exchange blacklists and wallet restrictions cannot substitute for consensus enforcement. The protocol defines no general-purpose WC transfer transaction.

Partial and full redemption are permitted. Settlement permanently burns the entire WC quantity selected and mints the corresponding NC at the applicable conversion rate. The burn and mint must succeed together. NC accounting must include WC liabilities, and the protocol cannot create guaranteed redemption or yield obligations without sufficient backing or reserved issuance capacity.

## Proposed mechanisms

Investigate a global conversion index so accrual can change the NC claim per WC unit without rewriting each developer account. Evaluate canonical update ordering, bounded arithmetic, aggregate liability, changes in eligibility, rounding and whether splitting redemptions changes total settlement. The proposed initial ratio is **5 WC to 1 NC**, pending ratification.

## Open decisions

WC precision, per-account cap and overflow handling, issuance and automatic yield formulas, conversion mathematics, index design, rounding and dust treatment, redemption authorization, minimums and eligibility after wallet changes. Resolve liability backing, updates and capacity sharing with [NC staking](MONETARY_PROTOCOL.md) before making guarantees. See [rewards](REWARDS.md).
