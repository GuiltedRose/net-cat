# Governance and adoption

## Established requirements

Founders, developers and investors receive no automatic network governance privileges. Fully open-source infrastructure is the objective. LegacyBound is the intended canonical source host. GitHub will remain an independent public mirror for accessibility, transparency and redundancy. Migration must preserve complete Git history. Software rights must remain independent of NC ownership, financial participation, network membership or a hosting provider.

LegacyBound should demonstrate practical utility before public cryptocurrency promotion. The purpose of cryptocurrency funding is NC liquidity and market adoption. Founder pay and software development financing are outside that objective. Public communication must avoid promises of returns, appreciation or staking yields.

No unilateral founder upgrades, universal recovery key, arbitrary balance seizure, hidden allocation, administrator minting or administrative bypass of monetary invariants is permitted. Founders and developers may use ordinary NC wallets. Mainnet genesis must be publicly disclosed and independently verifiable. Development and integration testing must work without privileged monetary authority.

BSD-3-Clause is the active license. The intended NCOSL transition depends on legal review, explicit adoption and documented rights-holder authorization. NCOSL should evolve in response to concrete development issues while preserving BSD-style freedoms. It is the intended ecosystem default, and hosted projects choose their own licenses. See the [contribution and licensing policy](../../README.md#licensing).

## Proposed mechanisms

Investigate reproducible builds, signed releases and independent audits. Evaluate a public change process with versioned proposals, disclosed decision criteria, interoperability review, and coordinated activation and recovery procedures. Assess how participants verify or reject upgrades without trusting a standing privileged party.

Investigate capture risks from validator concentration, randomness manipulation, malicious snapshots and attempts to override monetary controls. See the [security threat model](SECURITY.md#attack-scenarios). The provisional thresholds below require simulation. Executable upgrade authorization and emergency authority remain open.

## Open decisions

Governance representation and capture resistance, protocol upgrades and emergency handling, exchange integration, initial liquidity, regulatory review and final adoption of the [proposed license](../../LICENSE.draft.md).

## Two-chamber governance

Adopt hybrid on-chain governance.

### Monetary chamber

Eligible passive NC stakers.

Voting power is proportional to eligible committed NC.

### Consensus chamber

Eligible active validators.

Voting power is proportional to eligible bonded NC.

Include all eligible active validators rather than only the current rotating committee.

Both chambers must independently approve major protocol amendments.

WC has no governance authority.

Do not introduce a separate governance token.

## Provisional governance parameters

| Parameter | Value |
|---|---|
| Monetary chamber approval | Greater than two-thirds |
| Consensus chamber approval | Greater than two-thirds |
| Minimum participation per chamber | 50% of eligible voting power |
| Public proposal review | 14 days |
| Voting period | 7 days |
| Activation delay | 14 days |

These parameters remain subject to simulation.

Specify whether approval thresholds use participating or total eligible voting power before implementation.

Define abstentions, zero-participation conditions and exact quorum arithmetic.

## Governance safeguards

- Record voting eligibility at a finalized pre-voting snapshot.
- Lock voting collateral throughout the required governance period.
- Prevent the same NC from counting in both chambers.
- Require independent chamber approval.
- No founder veto.
- No unilateral administrator upgrades.
- Publicly auditable proposals and results.
- Delayed activation at a predetermined block height.
- Preserve monetary and security invariants.

Progressive staking burns increase the cost of obtaining governance influence but do not eliminate whale capture.

Simulate coordinated control of both chambers and governance deadlock.

Exact proposal admission, upgrade compatibility, executable activation, emergency handling and governance amendment rules remain technical specification requirements.
