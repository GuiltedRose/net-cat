# Developer identity

## Established requirements

Developer accounts exist independently of wallet addresses. Wallet linking is optional and proves wallet ownership cryptographically. Ordinary removal requires authorization from both the account and the linked wallet. Accounts without eligible linked wallets do not accrue monetary participation rewards. Pending earnings remain under on-chain account control until scheduled payout or redemption.

A wallet has no unilateral account authority. Independent recovery must replace a compromised wallet while preserving the developer account, repository ownership, contribution history and account-controlled funds. Financial and developer keys should be isolated where practical. Recovery has no universal key. WC and its per-account cap stay with the developer account across wallet changes. See [WetCat](WETCAT.md).

## Proposed mechanisms

Evaluate a stable account identifier, separate authorization roles and a recovery authority independent of the wallet being replaced. Bind link and removal signatures to the chain, account, wallet and specific action, with freshness and replay protection. Review recovery delays, notification and cancellation, and how payout routing responds while recovery is pending.

## Open decisions

Account control representation, eligible wallet proof, authority thresholds and recovery conflicts, recovery of developer authorization keys, handling pending earnings during wallet ineligibility, and synchronization with repository rights and payouts. See [rewards](REWARDS.md).
