# Native transaction fees

## Approved requirements

Native transaction fees must be extremely low. Congestion must not raise fees or allow priority bidding. All native network fees are permanently burned and reduce outstanding NC. See [supply accounting](MONETARY_PROTOCOL.md#effective-monetary-supply). Congestion affects processing availability and confirmation time.

## Design target

A standard transaction should cost below one US cent. A fixed NC fee cannot guarantee a USD ceiling at every possible NC valuation. The standard transaction class and measurement basis remain open.

## Proposed mechanisms

Investigate bounded admission and scheduling policies against spam, resource exhaustion, Sybil manipulation and censorship. Evaluate any market-price adjustment separately from congestion, including data source manipulation, stale or unavailable prices, deterministic validation and independence from mandatory external financial services.

## Open decisions

Fee calculation, fee-bearing operations, standard transaction definition, scheduling, spam resistance and whether to use a market-price adjustment. Ordinary [Git and collaboration operations](REPOSITORIES.md) require no cryptocurrency payment. How registration-related writes are submitted and funded remains open.
