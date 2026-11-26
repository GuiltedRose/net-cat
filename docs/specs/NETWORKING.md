# Networking

## Established requirements

P2P operation has no required central gateway. Meaningful participation should be feasible on ordinary consumer hardware and residential connections. Wire data uses compact deterministic binary serialization. Chain validation and repository storage can operate independently.

Support independent light clients, pruned full nodes, validators, archival nodes, repository storage nodes and passive stakers. Roles may overlap. Synchronize through authenticated finalized snapshots and respect configurable historical retention above protocol security minimums. The [ledger specification](LEDGER.md) defines the state and evidence pruning must preserve. Git contents remain off-chain.

## Networking framework

Provisionally adopt rust-libp2p with modular protocol support.

| Component | Protocol |
|---|---|
| Primary transport | QUIC |
| Fallback transport | TCP + Noise + Yamux |
| Peer discovery | Kademlia DHT |
| Gossip propagation | GossipSub |
| Direct data retrieval | Request-response |
| NAT detection | AutoNAT |
| NAT traversal | DCUtR |
| Relaying | Circuit Relay v2 |
| Local discovery | mDNS |

Enable only the components necessary for the node's configured roles.

Existing networking libraries should be preferred over custom transport implementations.

## Traffic prioritization

Separate logical traffic categories:

1. Consensus votes and block propagation.
2. Ledger synchronization, snapshots and proofs.
3. Repository data transfers.

Bulk repository transfers must not starve consensus.

Operators may configure repository bandwidth and storage budgets.

Transport encryption does not replace repository-level encryption or authorization.

## Provisional networking limits

| Parameter | Value |
|---|---|
| Target ordinary peer connections | 16 |
| Maximum ordinary peer connections | 64 |
| Minimum ordinary peer connections | None |
| Maximum block payload | 256 KiB |
| Snapshot chunk size | 4 MiB |
| Peer identity | Persistent cryptographic key |
| Network addresses | Changeable |

These values remain subject to benchmarking.

Validator-specific connectivity may require separate limits.

The maximum block payload requires a precise definition of included and excluded certificate and framing overhead.

## Peer discovery and bootstrapping

Support:

- Multiple independent bootstrap peers.
- Cached peer addresses.
- Manual peer configuration.
- Decentralized discovery.
- Replaceable bootstrap infrastructure.
- NAT-compatible connectivity where possible.

A node with zero peers may operate locally and listen for connections.

Sixteen peers is a connectivity target, not an operation requirement.

Single-validator operation is acceptable for development and testing.

Production decentralization must not be claimed merely because a single founder-operated validator can finalize blocks.

Initial production validator distribution and bootstrap trust assumptions remain launch requirements.

## Open specifications

Define message framing and limits, peer diversity and discovery trust, rate and resource budgets, consensus traffic isolation, and bootstrap recovery for residential links. Benchmark initial sync, catch-up, steady-state and validator connectivity. See [ledger](LEDGER.md).
