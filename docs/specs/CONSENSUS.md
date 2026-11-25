# Consensus

## Established requirements

Consensus must enforce deterministic ledger and monetary rules without mandatory central authority or standing insider privileges. Validator participation must be designed for consumer hardware and residential networks. Staking entry requires the monetary protocol's irreversible burn.

## Proposed mechanisms

Evaluate candidate consensus designs against explicit assumptions about network partitions, adversarial participation and recovery. Specify validation, fork choice, finality, equivocation evidence and validator-set changes before choosing an implementation. Model whether entry burns, separately slashable collateral and validator selection can support safety without excluding smaller participants.

## Open decisions

Consensus family, validator selection, admission and unbonding, collateral and penalties, finality and checkpoint trust, block sizes and intervals, and Sybil and whale resistance. See [monetary protocol](MONETARY_PROTOCOL.md), [ledger](LEDGER.md) and [governance](GOVERNANCE.md).
