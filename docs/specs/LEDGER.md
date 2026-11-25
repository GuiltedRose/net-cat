# Ledger

## Established requirements

Ledger state supports NC balances, developer account control and earnings, and verifiable repository registry changes. Git contents stay off-chain. Consensus data uses compact and deterministic binary encoding with exact integer monetary arithmetic. NC balances use the approved [monetary representation](MONETARY_PROTOCOL.md). WC is account-bound in consensus state and has no transfer operation. Its issuance and redemption must enforce [WC invariants](WETCAT.md).

## Proposed mechanisms

Specify canonical encodings with bounded lengths, explicit versioning, stable field ordering and rejection of ambiguous forms. Evaluate commitments to state, authenticated snapshots, pruned validators, light clients and separate archival nodes. Specify how a new node authenticates snapshot state and finality, and what evidence a light client can verify. Pruning must preserve the information needed for current validation, authorization and replay protection.

## Open decisions

Transaction and block formats, signing domains, state model and commitments, retention, snapshots and bootstrap trust, light-client proofs, block limits and state synchronization. See [consensus](CONSENSUS.md) and [wallet](WALLET.md).
