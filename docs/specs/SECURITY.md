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

## Attack scenarios

| Actor | Scenario and required design response |
| --- | --- |
| Malicious founder | Conceals a genesis allocation, introduces administrative minting, a developer tax, seizure or invariant bypass, or attempts unilateral activation. Require public genesis verification and protocol-enforced founder neutrality. |
| Compromised infrastructure | Serves a substituted release, fraudulent node state or stale repository authorization. Investigate reproducible builds, signed releases and independent audits. Verify state and access evidence independently. |
| Fraudulent contributor | Submits duplicate or fabricated evidence or uses high-volume AI-assisted activity to inflate rewards. Evaluate evidence provenance, deduplication, disputes and valuation of meaningful work. |
| Wealthy validator | Splits stake among identities, captures issuance or manipulates admission under congestion. Evaluate validator and issuance concentration jointly with scheduling rules that prohibit congestion pricing. |
| Compromised wallet | Tries to redirect a payout, bypass the WC account cap by reassociation or seize repository control. Require account-scoped authorization and independent recovery. |
