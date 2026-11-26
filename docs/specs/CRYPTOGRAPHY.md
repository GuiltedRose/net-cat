# Cryptography and compression

## Provisional cryptographic suite

| Operation | Selected candidate |
|---|---|
| Block hashing | BLAKE3-256 |
| Transaction hashing | BLAKE3-256 |
| Wallet signatures | Ed25519 |
| Validator signatures | Ed25519 |
| Verifiable randomness proofs | ECVRF-Edwards25519 |
| Repository key agreement | X25519 |
| Repository encryption | ChaCha20-Poly1305 |
| Key derivation | HKDF-SHA256 |
| State commitments | BLAKE3 Merkle trees |
| Historical compression | Zstandard |
| Git compression | Native Git packfiles |

BLS12-381 aggregate signatures remain a validator-specific benchmark candidate.

Do not silently replace Ed25519 with BLS.

Benchmark native storage size, verification throughput, memory consumption and synchronization bandwidth.

No custom cryptographic algorithms.

Separate wallet signing, validator voting, developer authorization, peer identity and repository encryption keys.

Specify domain separation for different operations.

Hashing, signatures and encrypted data generally do not benefit substantially from generic compression.

Prefer compact native binary representations.

## Outstanding cryptographic details

Retain explicit requirements for:

- Secure unbiased network randomness.
- Exact ECVRF ciphersuite.
- Repository key management.
- Authenticated encryption nonce handling.
- Key rotation and revocation.
- Hardware wallet compatibility.
- Signature canonicalization.
- Storage proofs.
- BLS aggregation security if adopted.
- Domain-separated commitments.
- Secure checkpoint authentication.

The selected primitive suite is a benchmark baseline, not a finalized cryptographic security proof.

Wallet, validator, developer, peer identity and repository key roles use separate signing or encryption contexts. See [consensus](CONSENSUS.md), [ledger](LEDGER.md), [wallet](WALLET.md) and [repositories](REPOSITORIES.md).
