# Ledger

## Established requirements

Ledger state supports NC balances, developer account control and earnings, and verifiable repository registry changes. Git contents stay off-chain. Consensus data uses compact and deterministic binary encoding with exact integer monetary arithmetic. NC balances use the approved [monetary representation](MONETARY_PROTOCOL.md). WC is account-bound in consensus state and has no transfer operation. Its issuance and redemption must enforce [WC invariants](WETCAT.md).

Native pruning and authenticated finalized state snapshots are required. Nodes may configure historical retention above protocol security minimums. Ordinary validators need no genesis-to-present archive. Independent archival nodes may retain historical data. Pruning must preserve active balances, authorization state, replay protection and necessary consensus evidence, and must leave monetary state and authorization outcomes unchanged. Git storage and pruning are independent. Consumer hardware and residential links set the resource targets.

## Canonical binary format

Use deterministic, versioned binary serialization.

Requirements:

- Canonical little-endian integer encoding.
- Fixed-width monetary integers.
- Raw binary hashes, keys and signatures.
- Compact typed transaction records.
- Canonical variable-length encoding where useful.
- Explicit framing and versioning.
- No compiler-dependent structure padding.
- No stored JSON, XML or hexadecimal representations in the canonical ledger.

Node APIs may generate human-readable JSON on demand.

Expose monetary amounts as decimal strings where necessary to prevent floating-point precision loss.

Binary encoding provides compactness, not transaction privacy.

## Proposed block header

| Field | Type | Bytes |
|---|---|---:|
| Protocol version | u16 | 2 |
| Block height | u64 | 8 |
| Timestamp | u64 | 8 |
| Consensus round | u32 | 4 |
| Previous block hash | BLAKE3-256 | 32 |
| Transaction Merkle root | BLAKE3-256 | 32 |
| State Merkle root | BLAKE3-256 | 32 |
| Committee commitment | BLAKE3-256 | 32 |
| Proposer index | u16 | 2 |
| Transaction count | u32 | 4 |
| Total | | 156 |

This is the preliminary header layout.

Additional consensus commitments, framing and metadata may alter the final encoded size.

Do not store a block hash redundantly inside its own hashed header.

Block structure:

- Header.
- Finality certificate.
- Typed transaction payload.
- Necessary authenticated consensus evidence.

All consensus-relevant block contents must be cryptographically committed.

## Proposed NC transfer record

| Field | Type | Bytes |
|---|---|---:|
| Transaction type | u8 | 1 |
| Sender public key | Ed25519 | 32 |
| Recipient identifier | 256-bit identifier | 32 |
| Amount | u64 | 8 |
| Fee | u64 | 8 |
| Replay-protection nonce | u64 | 8 |
| Signature | Ed25519 | 64 |
| Total | | 153 |

This is a preliminary payload size excluding additional framing, versioning and necessary authorization context.

The final signature format must bind transactions to their intended network and operation.

Do not claim these preliminary layouts are sufficient production wire formats.

## Specialized transaction types

NullCat does not require a general-purpose smart contract VM in its baseline design.

Define specialized transaction families for:

- NC transfers.
- Staking.
- Unstaking.
- Validator activation and deactivation.
- WC redemption.
- Developer account registration.
- Wallet association and recovery.
- Repository registration.
- Permission updates.
- Contribution settlement.
- Storage settlement.
- Governance proposals and voting.

Each transaction type should contain only the information necessary for independent validation.

## Active state and historical storage

Separate:

- Historical block ledger.
- Authenticated active state.
- State snapshots.
- Optional transaction indexes.
- Optional address indexes.
- Independent Git repository storage.

Nodes must validate new state transitions against authenticated active state rather than repeatedly searching transaction history.

Database updates must be atomic upon finalized block commitment.

The underlying database engine is an implementation detail.

Different conforming storage engines must produce identical canonical state commitments.

## Authenticated state architecture

Provisionally adopt BLAKE3-based compressed sparse Merkle trees.

Use separate namespaces for:

- Monetary accounting.
- WetCat.
- Staking.
- Consensus.
- Developer identity.
- Repository authorization.
- Governance.

Each namespace produces a 32-byte commitment.

Combine these into one canonical global 32-byte state root.

The design must support efficient updates, membership proofs and non-membership proofs.

Specify canonical namespace ordering, tree encoding, empty roots and domain separation before implementation.

## Snapshot synchronization

Synchronization procedure:

1. Discover independent peers.
2. Establish an authenticated recent consensus checkpoint.
3. Retrieve compressed snapshot chunks.
4. Verify snapshot data against the finalized state commitment.
5. Independently validate subsequent blocks.
6. Begin normal node operation.

Provisional snapshot parameters:

| Parameter | Value |
|---|---|
| Generation interval | 24 hours |
| Compression | Zstandard |
| Uncompressed chunk size | 4 MiB |
| Local snapshot retention | 2 |
| Chunk hashing | BLAKE3-256 |
| Parallel retrieval | Supported |
| Interrupted download recovery | Supported |

Snapshot providers have no privileged authority.

Enforce strict decompression and resource limits.

A Merkle root alone does not establish that a snapshot belongs to the legitimate chain.

Secure checkpointing and long-range attack protection remain required.

## Historical and pruned nodes

Support independently configurable node roles:

- Historical full node.
- Pruned full node.
- Light client.
- Active validator.
- Repository storage node.
- Passive staker.

Historical nodes may retain and verify complete chain history.

Pruned full nodes retain authenticated active state and required recent history.

Both historical and pruned full nodes may become validators.

Support optional genesis-to-present verification before pruning and snapshot-based initial synchronization.

Transaction and address indexing remain optional.

Operators may switch between historical and pruned modes, subject to retrieving any required historical data.

## Replication-aware pruning

Pruning is local, not network-wide deletion.

Pruning must never alter active monetary state or authorization outcomes.

State snapshots cannot reconstruct deleted transaction history.

Provisional historical replication parameters:

```text
target archival copies = 5

minimum other verified copies before automatic pruning = 3
```

When replication is insufficient:

- Suspend automatic pruning of affected historical segments.
- Notify the operator.
- Prioritize eligible under-replicated data in storage rewards.

These are provisional safety and incentive parameters.

Do not claim that replication proofs guarantee future availability or prove independent beneficial ownership of storage nodes.

Repository inactivity must not automatically trigger destruction of the final available copy.

## Infrastructure migration

The network must support:

- Hardware migration.
- Storage migration.
- IP address changes.
- Hosting provider changes.
- Peer identity persistence.
- Secure validator migration.
- Independent archival replication.

Validator migrations must prevent accidental simultaneous signing and preserve required signing-safety state.

The founder must be able to replace or upgrade infrastructure without interrupting the network once sufficient independent participation exists.

## Open specifications

Canonical framing, signing context, nonce semantics, tree and snapshot encoding, checkpoint authentication, retention minima, evidence windows, optional index behavior and snapshot limits require detailed specifications. Pruning must preserve replay protection and slashable evidence. See [cryptographic candidates](CRYPTOGRAPHY.md) and [storage](STORAGE.md).
