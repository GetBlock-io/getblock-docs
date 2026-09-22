---
description: >-
  GetBlock provides fast and reliable access to Akash nodes via the CometBFT
  JSON-RPC API. Connect to the Akash network without running your own
  infrastructure.
---

# JSON-RPC API - Akash

The CometBFT JSON-RPC interface for Akash: consensus and node status, block and transaction data, mempool, transaction broadcast, and `abci_query` for reading any Cosmos SDK or Akash module state. All methods are JSON-RPC 2.0 POST requests to the endpoint base URL.

### Akash specifics

* **Shared nodes are pruned.** Heights below roughly 26,980,292 are not retained and return `-32603 Internal error` naming the lowest available height. Read the floor from [status](status.md) under `sync_info.earliest_block_height`.
* **Hash encodings differ per method.** [block\_by\_hash](block_by_hash.md) and [tx](tx.md) take the hash **base64-encoded**, while [header\_by\_hash](header_by_hash.md) takes the **plain hex** form. A `0x` prefix is rejected by all three.
* **Wide queries time out.** A [block\_search](block_search.md) spanning millions of heights returns a gateway timeout; narrow the range and keep `per_page` small.

## Methods

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>status</td><td>Node, sync, and validator status</td></tr><tr><td>health</td><td>Node liveness check</td></tr><tr><td>net_info</td><td>Peer connections</td></tr><tr><td>block</td><td>Block by height (or latest)</td></tr><tr><td>block_results</td><td>Execution results and events for a block</td></tr><tr><td>validators</td><td>Validator set at a height</td></tr><tr><td>tx</td><td>Transaction result by hash</td></tr><tr><td>tx_search</td><td>Search transactions by event query</td></tr><tr><td>abci_query</td><td>Query any module state via ABCI</td></tr><tr><td>broadcast_tx_sync</td><td>Broadcast a transaction (CheckTx)</td></tr><tr><td>blockchain</td><td>Block headers in a height range</td></tr><tr><td>block_by_hash</td><td>Block by hash</td></tr><tr><td>commit</td><td>Commit (signed header) at a height</td></tr><tr><td>header</td><td>Block header at a height</td></tr><tr><td>header_by_hash</td><td>Block header by hash</td></tr><tr><td>genesis</td><td>Genesis document</td></tr><tr><td>genesis_chunked</td><td>Genesis document in chunks</td></tr><tr><td>consensus_params</td><td>Consensus parameters at a height</td></tr><tr><td>consensus_state</td><td>Current consensus state</td></tr><tr><td>dump_consensus_state</td><td>Detailed consensus state</td></tr><tr><td>unconfirmed_txs</td><td>Mempool transactions</td></tr><tr><td>num_unconfirmed_txs</td><td>Mempool transaction count</td></tr><tr><td>block_search</td><td>Search blocks by event query</td></tr><tr><td>abci_info</td><td>ABCI application info</td></tr><tr><td>check_tx</td><td>Check a transaction without broadcasting</td></tr><tr><td>broadcast_tx_async</td><td>Broadcast a transaction (fire-and-forget)</td></tr><tr><td>broadcast_tx_commit</td><td>Broadcast and wait for a block</td></tr><tr><td>broadcast_evidence</td><td>Submit misbehaviour evidence</td></tr></tbody></table>

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
