# Ledger

## Established requirements

Ledger state supports NC balances, developer account control and earnings, and verifiable repository registry changes. Git contents stay off-chain. Consensus data uses compact and deterministic binary encoding with exact integer monetary arithmetic. NC balances use the approved [monetary representation](MONETARY_PROTOCOL.md). WC is account-bound in consensus state and has no transfer operation. Its issuance and redemption must enforce [WC invariants](WETCAT.md).

Native pruning and authenticated finalized state snapshots are required. Nodes may configure historical retention above protocol security minimums. Ordinary validators need no genesis-to-present archive. Independent archival nodes may retain historical data. Pruning must preserve active balances, authorization state, replay protection and necessary consensus evidence, and must leave monetary state and authorization outcomes unchanged. Git storage and pruning are independent. Consumer hardware and residential links set the resource targets.

## Proposed mechanisms

Specify canonical encodings with bounded lengths, explicit versioning, stable field ordering and rejection of ambiguous forms. Evaluate state commitments, snapshot encodings, secure checkpointing, light-client proofs and historical data availability. Specify how a new node authenticates snapshot state and finality, and what evidence a light client can verify. Pruning must preserve the information needed for current validation, authorization and replay protection.

## Open decisions

Transaction and block formats, signing domains, state model and commitments, retention, snapshots and bootstrap trust, light-client proofs, block limits and state synchronization. See [consensus](CONSENSUS.md) and [wallet](WALLET.md).
