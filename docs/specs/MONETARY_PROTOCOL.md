# Monetary protocol

## Established requirements

NC is native and transferable without a developer account. The proposed cap is 100 trillion NC outstanding. Genesis mints a few million NC, with the amount and allocation open. Outstanding supply must stay within the cap even if cumulative minting exceeds it after valid burns reopen headroom. All network fees are burned. Rewards use deterministic minting with no dedicated prefunded pool, insider allocations, privileged wallets or developer tax.

Staking entry irreversibly burns a dynamic percentage of eligible holdings. The calculation depends on individual holdings and network monetary conditions. Design must preserve earning opportunities despite wealth concentration and consider holdings split across identities.

## Proposed mechanisms

Track outstanding NC as total valid minting minus total valid burns, regardless of custody, transfers, locks or lost keys. For each ordered transition, enforce `0 <= outstanding <= cap`. Apply burns and mints atomically when a transition requires both. Define exact atomic-unit arithmetic for every state transition and an explicit order for competing claims. A potential headroom reservation for guaranteed, unminted claims would additionally enforce `outstanding + reserved <= cap`. Already-minted NC held for payout counts in outstanding supply and must not also count as unminted reserved headroom.

An optional account-bound secondary asset could be earned through eligible participation, may accumulate redeemable value and would have a variable NC conversion ratio. A liquid staking asset's changing exchange rate against SOL is an analogy for valuation only. The secondary asset would have no independent exchange listing. Redemption would atomically destroy the redeemed units and deliver NC. Any NC minted must fit under the cap, and guaranteed liabilities need explicit backing or headroom treatment at issuance and revaluation.

## Open decisions

Cap ratification, genesis, precision, growth and regenerative issuance, fee and burn math, holdings measurement, staking rewards, secondary asset adoption, name, issuance, valuation and redemption. See [reward accounting](REWARDS.md) and [consensus](CONSENSUS.md).
