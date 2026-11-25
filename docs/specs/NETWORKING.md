# Networking

## Established requirements

P2P operation has no required central gateway. Meaningful participation should be feasible on ordinary consumer hardware and residential connections. Wire data uses compact deterministic binary serialization. Chain validation and repository storage can operate independently.

## Proposed mechanisms

Evaluate multiple discovery paths and inbound/outbound peer diversity. Bound message sizes, in-flight work, queues, peer rates and verification cost. Separate ledger synchronization from Git replication so storage demand cannot exhaust validation resources. Benchmark initial sync, steady-state transfer, disk growth and catch-up under published hardware and link profiles.

## Open decisions

Discovery, transport, peer diversity, routing and relay policy, upload and download budgets, CPU and memory limits, sync performance targets and handling of interrupted residential links. See [ledger](LEDGER.md) and [repositories](REPOSITORIES.md).
