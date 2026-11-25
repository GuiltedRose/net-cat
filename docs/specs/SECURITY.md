# Security

## Established requirements

Use established cryptographic algorithms. Keep wallet and recovery secrets local. Require cryptographic authorization for account and repository state changes, verify private repository access at the supplier and preserve monetary invariants under malformed or adversarial input.

## Proposed verification criteria

| Surface | Required assurance to specify and test before release |
| --- | --- |
| Money and rewards | Bound supply, burns, minting and liabilities across cap contention, overflow, replay and partial failures. |
| Identity and wallets | Exercise compromised wallet replacement, replay across chains or action types, double payout and takeover attempts against recovery. |
| Private Git | Deny stale, forged or missing authorization and test revocation, key rotation and metadata leakage. |
| Consensus and sync | Test conflicting histories, snapshots with invalid commitments, withheld evidence, partitions and recovery assumptions. |
| Network and parsers | Exercise malformed inputs, resource exhaustion and peer capture against documented limits. |
| Governance | Evaluate upgrade compatibility, activation failure and recovery authority against capture. |

## Open decisions

Threat assumptions, primitives and key lifecycle, assurance criteria, incident handling and release recovery authority. See [open questions](../OPEN_QUESTIONS.md).
