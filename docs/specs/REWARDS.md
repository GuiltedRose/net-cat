# Participation rewards

## Established requirements

Rewards are deterministic protocol-controlled minting, with no dedicated prefunded reward pool. Developers participate under the same rules as everyone else. Developer accounts without an eligible linked wallet do not accrue monetary participation rewards. Pending earnings remain under account-level control until scheduled payout or redemption. Native NC transfers and ordinary network participation are available without LegacyBound.

## Proposed mechanisms

Separate the evidence used to recognize a contribution from the deterministic transition that settles its reward. Evaluate verifiable evidence, duplicate suppression, versioned valuation and a dispute process. Record claim state and settlement so payouts cannot repeat and wallet replacement does not change account ownership of pending earnings. Define when a claim becomes a monetary liability and how the cap constrains accrual, minting and redemption.

## Open decisions

Contribution eligibility and valuation, evidence authority and disputes, payout cycle, staking reward calculations, accrual timing, cap contention, and routing and settlement during wallet removal or recovery. Cryptographic CTF challenges are outside the current scope. See [monetary protocol](MONETARY_PROTOCOL.md).
