---
description: >-
  GetBlock provides fast and reliable access to Akash nodes via the CometBFT
  JSON-RPC API. Connect to the Akash network without running your own
  infrastructure.
---

# JSON-RPC API

The CometBFT JSON-RPC interface for Akash: consensus and node status, block and transaction data, mempool, transaction broadcast, and `abci_query` for reading any Cosmos SDK or Akash module state. All methods are JSON-RPC 2.0 POST requests to the endpoint base URL.

### Akash specifics

* **Shared nodes are pruned.** Heights below roughly 26,980,292 are not retained and return `-32603 Internal error` naming the lowest available height. Read the floor from [status](status.md) under `sync_info.earliest_block_height`.
* **Hash encodings differ per method.** [block\_by\_hash](block_by_hash.md) and [tx](tx.md) take the hash **base64-encoded**, while [header\_by\_hash](header_by_hash.md) takes the **plain hex** form. A `0x` prefix is rejected by all three.
* **Wide queries time out.** A [block\_search](block_search.md) spanning millions of heights returns a gateway timeout; narrow the range and keep `per_page` small.

## Methods

| Method                 | Description                               |
| ---------------------- | ----------------------------------------- |
| status                 | Node, sync, and validator status          |
| health                 | Node liveness check                       |
| net\_info              | Peer connections                          |
| block                  | Block by height (or latest)               |
| block\_results         | Execution results and events for a block  |
| validators             | Validator set at a height                 |
| tx                     | Transaction result by hash                |
| tx\_search             | Search transactions by event query        |
| abci\_query            | Query any module state via ABCI           |
| broadcast\_tx\_sync    | Broadcast a transaction (CheckTx)         |
| blockchain             | Block headers in a height range           |
| block\_by\_hash        | Block by hash                             |
| commit                 | Commit (signed header) at a height        |
| header                 | Block header at a height                  |
| header\_by\_hash       | Block header by hash                      |
| genesis                | Genesis document                          |
| genesis\_chunked       | Genesis document in chunks                |
| consensus\_params      | Consensus parameters at a height          |
| consensus\_state       | Current consensus state                   |
| dump\_consensus\_state | Detailed consensus state                  |
| unconfirmed\_txs       | Mempool transactions                      |
| num\_unconfirmed\_txs  | Mempool transaction count                 |
| block\_search          | Search blocks by event query              |
| abci\_info             | ABCI application info                     |
| check\_tx              | Check a transaction without broadcasting  |
| broadcast\_tx\_async   | Broadcast a transaction (fire-and-forget) |
| broadcast\_tx\_commit  | Broadcast and wait for a block            |
| broadcast\_evidence    | Submit misbehaviour evidence              |

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
