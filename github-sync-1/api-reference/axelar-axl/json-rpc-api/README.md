---
description: >-
  GetBlock provides fast and reliable access to Axelar nodes via the CometBFT
  JSON-RPC API. Connect to the Axelar network without running your own
  infrastructure.
---

# JSON-RPC API

The CometBFT JSON-RPC interface for Axelar: consensus and node status, block and transaction data, mempool, transaction broadcast, and `abci_query` for reading any Cosmos SDK or Axelar module state. All methods are JSON-RPC 2.0 POST requests to the endpoint base URL.

## Methods

| Method                 | Description                               |
| ---------------------- | ----------------------------------------- |
| `status`               | Node, sync, and validator status          |
| `health`               | Node liveness check                       |
| `net_info`             | Peer connections                          |
| `block`                | Block by height (or latest)               |
| `block_results`        | Execution results and events for a block  |
| `validators`           | Validator set at a height                 |
| `tx`                   | Transaction result by hash                |
| `tx_search`            | Search transactions by event query        |
| `abci_query`           | Query any module state via ABCI           |
| `broadcast_tx_sync`    | Broadcast a transaction (CheckTx)         |
| `blockchain`           | Block headers in a height range           |
| `block_by_hash`        | Block by hash                             |
| `commit`               | Commit (signed header) at a height        |
| `header`               | Block header at a height                  |
| `header_by_hash`       | Block header by hash                      |
| `genesis`              | Genesis document                          |
| `genesis_chunked`      | Genesis document in chunks                |
| `consensus_params`     | Consensus parameters at a height          |
| `consensus_state`      | Current consensus state                   |
| `dump_consensus_state` | Detailed consensus state                  |
| `unconfirmed_txs`      | Mempool transactions                      |
| `num_unconfirmed_txs`  | Mempool transaction count                 |
| `block_search`         | Search blocks by event query              |
| `abci_info`            | ABCI application info                     |
| `check_tx`             | Check a transaction without broadcasting  |
| `broadcast_tx_async`   | Broadcast a transaction (fire-and-forget) |
| `broadcast_tx_commit`  | Broadcast and wait for a block            |
| `broadcast_evidence`   | Submit misbehaviour evidence              |

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
