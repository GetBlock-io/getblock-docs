---
description: >-
  GetBlock provides fast and reliable access to Dash nodes via the Blockbook
  WebSocket API. Connect to the Dash network without running your own
  infrastructure.
---

# Dash Blockbook (Websocket) API

The Blockbook indexer WebSocket interface for Dash: request/response queries (status, account info, transactions, broadcast, fee estimation) plus subscriptions to new blocks and address activity over a persistent connection.

## Methods

* [getInfo](/broken/pages/df274357ae03297262e0861a3c7bdd99550872c8) — Indexer and backend status
* [getAccountInfo](/broken/pages/096ef7fc9d949189ca86e1ee8241a6b2770413f5) — Address balance and history (WS)
* [getTransaction](/broken/pages/3853375ec7f9e61af6d7f42477ae45a3f9bdf12f) — Transaction by txid (WS)
* [sendTransaction](/broken/pages/aeb30791c81d7a6ac65beaae6928e56c53127f25) — Broadcast a transaction (WS)
* [estimateFee](/broken/pages/d023246893369494d5d1796713234b0ac1bb9cbe) — Fee estimate (WS)
* [subscribeNewBlock](/broken/pages/e03182b5af49feec90f17fcb7bd5d2c1239f4f68) — Subscribe to new blocks (WS)
* [subscribeAddresses](/broken/pages/d8a8ff5c82f0f5ea9ca41aa3ff6415a96b064767) — Subscribe to address activity (WS)

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
