# Repository registry and storage

## Established requirements

Public and private repositories may register on-chain. Signed records make ownership and permission changes verifiable. Public Git data is retrievable without authorization. Private Git data is encrypted and distributed only to explicitly authorized requesters. Each supplying node verifies authorization independently before serving any private content, including ciphertext. Git data travels over P2P storage and transfer infrastructure. Revocation prevents future authorized synchronization and cannot erase copies already downloaded.

Repository creation, cloning, pushes, issues, pull requests, code reviews and ordinary access require no cryptocurrency payment. Private access still requires authorization. LegacyBound will use an established Git implementation and remain compatible with ordinary Git clients. Migration must preserve full history, commit hashes, branches, tags and signatures. Repository owners choose their own licenses.

Imported Git history earns no retroactive rewards. Repository storage operates independently of consensus, supports configurable budgets, and is pruned separately from the ledger. Inactivity alone must never trigger network-wide deletion of the last available copy. See [storage rewards](STORAGE.md).

## Proposed mechanisms

Compare gitoxide, libgit2 and system Git for client interoperability, history preservation, signed objects and deployment trade-offs. Evaluate how registry updates can be submitted and funded without requiring users to pay for ordinary collaboration.

Evaluate commitments or opaque references for private registry metadata and permission proofs. Bind each private transfer's authorization to the requester, repository and current authorized state. Refuse private transfers when freshness or finality evidence is insufficient. Evaluate revocation checks during long transfers and key rotation for subsequent content. Analyze discovery and traffic leakage from identifiers, access patterns and replication.

## Open decisions

Git implementation and migration verification, fee-free creation and registry submission, metadata and access proof formats, authorization freshness and revocation semantics, key distribution and rotation, Git object references, replica enrollment, eligibility and final-copy detection, and durability when all eligible replicas are offline. See [identity](IDENTITY.md) and [security](SECURITY.md).

## Engineering baseline

Git packfiles remain native. X25519, ChaCha20-Poly1305 and HKDF-SHA256 are benchmark candidates. Key distribution, nonce handling, rotation and revocation require specification. Transport security does not replace repository encryption or supplier-side authorization using fresh finalized state. See [cryptography](CRYPTOGRAPHY.md).
