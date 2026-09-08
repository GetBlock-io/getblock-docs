---
description: >-
  GetBlock provides fast and reliable access to Dash nodes via REST API. Connect
  to the Dash network without running your own infrastructure.
---

# Dash REST API

The Bitcoin Core-style REST interface for Dash: read-only HTTP GET endpoints under `/rest/` for chain info, blocks, headers, transactions, and the UTXO set.

## Methods

| Method              | Description                      |
| ------------------- | -------------------------------- |
| `chaininfo`         | Chain state (REST)               |
| `tx`                | Transaction by txid (REST)       |
| `block`             | Block by hash (REST)             |
| `block-notxdetails` | Block header + txids (REST)      |
| `headers`           | Block headers from a hash (REST) |
| `getutxos`          | UTXO set lookup (REST)           |

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
