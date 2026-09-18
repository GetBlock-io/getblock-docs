---
description: >-
  GetBlock provides fast and reliable access to Dash nodes via the Blockbook
  WebSocket API. Connect to the Dash network without running your own
  infrastructure.
---

# Dash Blockbook (Websocket) API

The Blockbook indexer WebSocket interface for Dash: request/response queries (status, account info, transactions, broadcast, fee estimation) plus subscriptions to new blocks and address activity over a persistent connection.

## Methods

* [getInfo](getinfo-dash.md) — Indexer and backend status
* [getAccountInfo](getaccountinfo-dash.md) — Address balance and history (WS)
* [getTransaction](gettransaction-dash.md) — Transaction by txid (WS)
* [sendTransaction](sendtransaction-dash.md) — Broadcast a transaction (WS)
* [estimateFee](estimatefee-dash.md) — Fee estimate (WS)
* [subscribeNewBlock](subscribenewblock-dash.md) — Subscribe to new blocks (WS)
* [subscribeAddresses](subscribeaddresses-dash.md) — Subscribe to address activity (WS)

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
