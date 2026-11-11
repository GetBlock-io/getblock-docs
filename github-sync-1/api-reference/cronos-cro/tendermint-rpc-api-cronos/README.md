---
description: >-
  JSON-RPC API reference for the Cronos Tendermint (CometBFT) RPC. Explore
  method list, request examples, and how to connect to GetBlock's Cronos RPC
  endpoints
---

# Tendermint RPC API - Cronos

The CometBFT JSON-RPC interface for Cronos: consensus and node status, block and transaction data, mempool inspection, transaction broadcast, and `abci_query` for reading any module's state. All methods are JSON-RPC 2.0 POST requests to the endpoint base URL.

## Methods

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td><code>status</code></td><td>Node, sync, and validator status</td></tr><tr><td><code>health</code></td><td>Node liveness check</td></tr><tr><td><code>net_info</code></td><td>Peer connections</td></tr><tr><td><code>block</code></td><td>Block by height (or latest)</td></tr><tr><td><code>block_results</code></td><td>Execution results and events for a block</td></tr><tr><td><code>validators</code></td><td>Validator set at a height</td></tr><tr><td><code>tx</code></td><td>Transaction result by hash</td></tr><tr><td><code>tx_search</code></td><td>Search transactions by event query</td></tr><tr><td><code>abci_query</code></td><td>Query any module state via ABCI</td></tr><tr><td><code>broadcast_tx_sync</code></td><td>Broadcast a transaction (CheckTx only)</td></tr></tbody></table>

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
