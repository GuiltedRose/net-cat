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
| D-012 | Ratify the 100 trillion NC cap and five decimal places with `u64` balances and checked `u128` intermediates | Supersedes D-002's provisional cap and v0.2.0's open NC precision. Five is the maximum precision that fits the whole cap in eight bytes. Lifetime issuance must be representable beyond `u64`. |
| D-013 | Adopt WetCat as the consensus-restricted account-bound asset | Supersedes v0.2.0's optional unnamed asset proposal. The proposed 5 WC to 1 NC initial ratio remains pending. |
| D-014 | Separate voluntary NC staking from WC earnings and require issuance capacity for both | Extends D-003 while keeping staking mechanics open. |
| D-015 | Require low burned fees with no congestion price or priority bids and free ordinary collaboration | Extends D-002. The sub-cent standard transaction goal is a target. |
| D-016 | Preserve Git interoperability and complete migration history. Measure meaningful work and batch reward evidence and settlement where practical | Extends D-005 and reduces unnecessary writes and activity-based reward gaming. |
| D-017 | Prioritize native centralized-exchange integration for initial market access | Refines D-009. Exchange targets are prospective and operation remains independent of external services. |
| D-018 | Prohibit administrative monetary bypasses and unilateral upgrades, require verifiable genesis and test without privileged authority | Extends D-003 and D-008 to reduce insider abuse. |
| D-019 | Make LegacyBound the intended canonical source host and retain GitHub as an independent public mirror | Supersedes v0.2.0's characterization of GitHub as temporary infrastructure. |
| D-020 | Retain BSD-3-Clause and intend NCOSL as the future ecosystem default subject to review and authorization | Extends D-011. Hosted repositories keep freedom of license choice. |
