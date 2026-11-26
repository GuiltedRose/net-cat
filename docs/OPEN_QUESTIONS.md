# Open questions

[Whitepaper v0.4.0](../WHITEPAPER.md) and [decisions D-021 through D-028](DECISIONS.md) record approved architecture. Outstanding choices require explicit approval. Simulation coefficients are provisional.

## 1. Monetary mathematics

- Define annual-to-period budgets, reference supply, ordering, reset and smoothing timing, initial utilization, normalization, manipulation resistance, monetary pressure and stability metrics, the independent stability constraint and handling of zero denominators.
- Specify arithmetic scales, rounding, overflow strategies and representability at aggregate and individual boundaries. Reconcile actual minting and effective issuance when settling WC liabilities, including already-minted payment value, repeated partial redemption, dust and simultaneous claims.
- Specify the WC appreciation index and cap adjustment formula, award valuation, and treatment when appreciation or a lower cap takes an existing balance above the limit.
- Define eligible holdings, observation windows, concentration measurement, burn initialization and rounding, time weighting, passive withdrawal, minimum participation, compounding, payout rules and reward precision.
- Define exact native fees, standard transaction class and USD measurement, scheduling, spam resistance and any independent market-price adjustment.

## 2. Consensus

- Specify committee size, rotation, voting power, selection mathematics, Byzantine assumptions and safety and liveness across locking, round changes, partitions, long-range attacks and committee transitions.
- Define validator reward scoring, collateral, malicious behavior evidence, slashing amounts, inactivity thresholds, unbonding periods and block timing.

## 3. Cryptographic primitives

- Select and review primitives for transaction signatures and domain separation, committee randomness, proofs, commitments, snapshots and private repository encryption and authorization.

## 4. Ledger and networking

- Specify transaction and block encodings, state commitments, snapshot formats and authentication, secure checkpointing, retention minima, evidence preservation, active state growth, historical availability and light-client verification.
- Define discovery, transport, routing, resource budgets and sync targets for consumer hardware and residential links.

## 5. Repository and storage verification

- Evaluate Git integrations and full-history migration verification. Define fee-free registry submission, private metadata, authorization freshness, revocation, key distribution and data routing.
- Specify storage proof and eligibility mechanisms, availability and retrieval scoring, replication targets, anti-farming, duplicate suppression and final-copy detection.
- Define contribution scoring, evidence, attribution, duplicate prevention, batching, payout cadence and disputes.

## 6. Identity and wallet security

- Specify account authorization, recovery thresholds and conflict handling, wallet eligibility transitions, pending earnings when a wallet is ineligible, and payout routing during recovery.
- Define keys and addresses, hardware signing payloads, client verification, chain-specific recovery and browser boundaries.

## 7. Governance

- Define representation, voting and upgrade authorization, activation, emergency authority, capture resistance, incident handling, release signing, reproducibility and independent audits.

## 8. Genesis

- Specify distribution of the approved 50 million NC mint, independent public verification and initial validator bootstrapping.

## 9. Economic simulation

- Specify reproducible scenario inputs, calibration and acceptance criteria for concentration, issuance sustainability, redemptions, storage, feedback stability and arithmetic safety. See [simulation requirements](specs/SIMULATION.md).

## 10. Market integration and legal review

- Define utility criteria, exchange interfaces and reconciliation, initial liquidity, listing process, optional wallet conversion and regulatory review.
- Obtain professional review and rights-holder authorization before explicit NCOSL adoption. BSD-3-Clause remains active and hosted repository owners choose their own licenses.
