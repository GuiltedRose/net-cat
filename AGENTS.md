# NullCat contributor rules

- Develop from approved specifications. Obtain explicit approval for unresolved protocol decisions and before starting production implementation for the affected area.
- Record architectural decisions permanently in `docs/DECISIONS.md`. Append decisions that supersede prior entries while retaining the earlier record.
- Update affected specifications, the whitepaper and the open-questions register when protocol behavior or design status changes. Review cross-document consistency.
- Keep established requirements, proposals and unresolved decisions distinct. Never silently assign monetary or consensus parameters.
- Prioritize correctness, security, decentralization and low resource use ahead of throughput.
- Use exact integer arithmetic for consensus-critical monetary calculations. Specify bounds, overflow handling and rounding before implementation.
- Use established cryptographic algorithms. Never commit private keys, wallet seeds or credentials.
- Require automated coverage for monetary and security invariants, including malformed input, unauthorized actions and boundary cases. Report actual validation.
- Never introduce privileged developer accounts, founder allocations, developer taxes or monetary bypasses.
- Keep changes focused and reads limited to relevant files. The operator runs builds, tests, activation and rollout. Supply the exact applicable commands and preserve existing data, budgets, risk controls and authorization.
