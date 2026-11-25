# Wallet integration

## Established requirements

NullCat Wallet is a separate non-custodial project. The first client is a browser extension, with possible later native apps. It supports NC natively plus BTC, ETH and SOL through their native networks. A dedicated NC Ledger app is planned. Existing Ledger apps may serve supported external currencies. Secrets stay on the client or hardware device and are never sent to nodes.

## Proposed mechanisms

Design canonical bounded signing payloads for embedded verification and clear signing. Evaluate device rendering of chain, operation, recipient, amount and fees, and protections against mixing NC transfers with account-link or repository authorization signatures. Specify what ledger evidence a client must verify before displaying balances, confirming receipt or signing transactions.

## Integration constraints

Wallet integration must respect consensus restrictions on WC transfers, trading and bridging. Future market-based NC-to-USDC conversion and external settlement are optional proposals. See [market integration](MARKET_INTEGRATION.md).

## Open decisions

Key and address conventions, transaction payloads, browser permission boundaries, client verification, device integration, recovery and chain-specific signing and broadcast behavior. See [ledger](LEDGER.md) and [identity](IDENTITY.md).
