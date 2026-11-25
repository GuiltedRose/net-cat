# Architectural decision log

Permanent record of established project constraints. Append subsequent approvals with rationale, affected specifications and superseded entries. Proposed mechanisms appear in the specifications and are tracked in [open questions](OPEN_QUESTIONS.md).

| ID | Established direction | Rationale |
| --- | --- | --- |
| D-001 | Native Rust chain, separate forge and wallet repositories | Keep monetary infrastructure independent of application operators and wallet clients. |
| D-002 | Hard limit on outstanding NC with a proposed ceiling of 100 trillion NC, fee burns and deterministic reward minting | Bound monetary supply while allowing future issuance after burns. |
| D-003 | Dynamic staking entry burn and participant-neutral monetary rules | Preserve earning access and avoid privileged developer and founder claims. |
| D-004 | P2P operation, compact encoding and consumer-hardware participation | Reduce barriers to independent validation and data distribution. |
| D-005 | On-chain repository registry and independent off-chain Git storage, with supplier-side private authorization | Verify control changes without storing Git objects on-chain. |
| D-006 | Developer accounts independent of optional wallets, with account-level claims and independent recovery | Preserve ownership and earnings across wallet loss or compromise. |
| D-007 | Browser-first non-custodial wallet, native chain support and eventual Ledger signing | Keep secrets local and external currency activity on its native network. |
| D-008 | Permissive licensing objective and governance without automatic insider privileges | Preserve software freedoms and equal protocol treatment. |
| D-009 | Demonstrate forge utility before public promotion and pursue liquidity for adoption | Separate market adoption from founder compensation and development finance. |
| D-010 | Documentation and legal drafting before implementation | Review dependencies and approve relevant specifications before committing to protocol behavior. |
| D-011 | BSD-3-Clause active, with NCOSL pending professional review, rights-holder authorization and explicit adoption | Establish standard permissive terms now and preserve contributor control over any future relicensing. |
