# Consensus

## Approved architecture

NullCat uses permissionless Proof of Stake and rotating validator committees with Tendermint-style Byzantine Fault Tolerant finality. Committee selection is cryptographically randomized and stake-weighted. Finalization requires **greater than two-thirds** of committee voting power. Nodes independently verify transactions and state transitions. Passive staking is separate from active validation. Active validators maintain separate slashable bonded NC, with no founder-controlled or privileged class.

Occasional downtime costs rewards. Prolonged inactivity may remove eligibility, and provable malicious consensus behavior may result in slashing. Validator withdrawals require a suitable unbonding period. The combined design requires its own safety and liveness analysis covering Byzantine fault assumptions, locking, round changes, committee transitions, randomness manipulation, partitions and long-range attacks. Committee sampling and Tendermint-style voting alone do not establish those guarantees.

Validators must support pruned operation. The security model must specify assumptions about Byzantine voting power in sampled committees, network timing and partition recovery, and how independently verified finalized state survives committee transitions and long-range attacks. The system requires analysis of selection bias, concentration and randomness manipulation.

## Open decisions

Committee size, voting-power assignment, selection and rotation mathematics and periods, collateral amounts, slashing percentages and evidence, unbonding duration, inactivity thresholds, block intervals, cryptographic and randomness primitives, checkpoint trust and genesis bootstrapping remain open. Prioritize ordinary consumer hardware and residential connectivity. See [ledger](LEDGER.md) and [security](SECURITY.md).
