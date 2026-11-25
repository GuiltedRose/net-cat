# Monetary protocol

## Approved NC representation and supply

| Parameter | Value |
| --- | --- |
| Maximum outstanding supply | 100 trillion NC |
| Decimal precision | 5 |
| Smallest unit | 0.00001 NC |
| Atomic units per NC | 100,000 |
| Maximum outstanding atomic units | 10,000,000,000,000,000,000 |
| Individual NC balance | `u64` |
| Intermediate arithmetic | Checked `u128` where required |

Five is the greatest decimal precision at which the full cap fits in an eight-byte balance. One more decimal place would exceed `u64`. Consensus-critical monetary operations use integers. Cumulative lifetime issuance must be representable beyond `u64`. Its representation and overflow semantics remain open.

NC is native and transferable without a developer account. Genesis should mint a few million NC. The precise amount and allocation remain open and the mainnet distribution must be public and independently verifiable. A valid burn reopens issuance headroom, allowing cumulative issuance to exceed the outstanding cap. All native fees burn. Rewards use deterministic minting, with no dedicated prefunded pool, hidden allocations, automatic founder rewards, privileged wallets, administrative minting or developer tax.

## Approved staking requirements

Holders may voluntarily stake raw NC independently of earning WC. Entry irreversibly burns a dynamic proportion of eligible holdings based on those holdings and network monetary conditions. The mechanism must discourage wealth from monopolizing future issuance and apply the same rules to founders, developers and investors. Issuance policy must allow staking and WC automatic accrual to coexist without either consuming all available capacity.

## Proposed accounting

Track outstanding NC as valid mints minus burns. Locks and changes in custody leave outstanding unchanged. Enforce `0 <= outstanding <= cap` at each committed transition. Evaluate reservations for guaranteed unminted liabilities with `outstanding + reserved <= cap`. Already-minted funds count in outstanding and must not also count as unminted reserves. Existing restrictions on prefunded reward pools apply to any backing proposal. Specify settlement ordering and failure semantics across reward issuance and [WC redemption](WETCAT.md).

## Open decisions

Exact genesis, lifetime issuance representation, growth and regenerative issuance, burn inputs and formula, reward allocation and denomination, collateral, validator relationship, staking duration and withdrawal conditions, and how capacity is shared and rationed. See [fees](FEES.md), [rewards](REWARDS.md) and [consensus](CONSENSUS.md).
