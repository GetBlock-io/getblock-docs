---
description: >-
  GetBlock provides fast and reliable access to Stable nodes via the Tendermint
  RPC API. Connect to the Stable network without running your own
  infrastructure.
---

# Tendermint RPC

The CometBFT (Tendermint) JSON-RPC interface for Stable: node and consensus info, block and transaction reads, mempool inspection, transaction broadcast, low-level `abci_query` state reads, and WebSocket event subscriptions. The same methods are available over HTTP (JSON-RPC POST or URI GET) and over WebSocket for subscriptions.

## Endpoints

{% tabs %}
{% tab title="HTTP" %}
```bash
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="WebSocket" %}
```bash
wss://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/websocket
```
{% endtab %}
{% endtabs %}

## Available API Methods

### Node Info & Health

| Method    | Description                                                                                                                                                                   |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| health    | Returns an empty result if the node is healthy; otherwise an error                                                                                                            |
| status    | Returns node information: protocol and software version, network (chain id), latest block hash, app hash, block height and time, and the node's validator info and sync state |
| net\_info | Returns peer-to-peer network information: the number of connected peers, listeners, and per-peer connection details                                                           |

### Blocks & Chain

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>blockchain</td><td>Returns block metadata (headers) for a range of heights, newest first</td></tr><tr><td>block</td><td>Returns the block at a given height, including its header, data (transactions), and commit</td></tr><tr><td>block_by_hash</td><td>Returns a block by its hash</td></tr><tr><td>block_results</td><td>Returns the results of all transactions in a block: per-tx execution results, and the block's finalize-block events and validator/consensus-param updates</td></tr><tr><td>commit</td><td>Returns the commit (validator signatures) for a block at a given height, with a flag indicating whether it is canonical</td></tr><tr><td>header</td><td>Returns the block header at a given height</td></tr><tr><td>header_by_hash</td><td>Returns a block header by its hash</td></tr></tbody></table>

### Consensus & Validators

| Method                 | Description                                                                                                           |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------- |
| validators             | Returns the validator set at a given height, paginated                                                                |
| consensus\_state       | Returns the current consensus state (round, step, and vote state) of the node                                         |
| dump\_consensus\_state | Returns the full consensus state: the node's round state plus the state of all peers                                  |
| consensus\_params      | Returns the consensus parameters (block size, evidence, and validator params) at a given height                       |
| genesis                | Returns the genesis document                                                                                          |
| genesis\_chunked       | Returns the genesis document in chunks, for chains (like Stable) whose genesis is too large to return in one response |

### Transactions

| Method                | Description                                                                                                                            |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| tx                    | Returns a transaction by its hash, with its execution result and (optionally) a Merkle inclusion proof                                 |
| tx\_search            | Searches transactions by an event-attribute query, with pagination and ordering                                                        |
| block\_search         | Searches blocks by an event-attribute query (over finalize-block events), with pagination and ordering                                 |
| broadcast\_tx\_sync   | Broadcasts a transaction and returns after CheckTx (mempool validation), without waiting for the transaction to be included in a block |
| broadcast\_tx\_async  | Broadcasts a transaction and returns immediately, without running CheckTx or waiting for inclusion                                     |
| broadcast\_tx\_commit | Broadcasts a transaction and waits until it is committed in a block, returning both the CheckTx and the execution results              |
| check\_tx             | Runs a CheckTx against the application for a transaction without broadcasting it, returning the validation result                      |

### Mempool

| Method                | Description                                                                                     |
| --------------------- | ----------------------------------------------------------------------------------------------- |
| unconfirmed\_txs      | Returns transactions currently in the mempool, up to an optional limit                          |
| num\_unconfirmed\_txs | Returns the count and total size of transactions in the mempool, without the transaction bodies |

### ABCI Application Queries

| Method      | Description                                                                                                           |
| ----------- | --------------------------------------------------------------------------------------------------------------------- |
| abci\_info  | Returns information about the ABCI application: its version and the last block height and app hash it has committed   |
| abci\_query | Runs an ABCI query against the application at a given path, optionally at a historical height and with a Merkle proof |

### WebSocket Subscriptions

| Method           | Description                                                                                |
| ---------------- | ------------------------------------------------------------------------------------------ |
| subscribe        | Subscribes to events matching a Tendermint event query, over the WebSocket endpoint        |
| unsubscribe      | Cancels a specific subscription on the WebSocket connection, identified by its event query |
| unsubscribe\_all | Cancels all subscriptions on the current WebSocket connection                              |

## See Also

* [Cosmos overview](../../cosmos/)
* [CometBFT RPC Reference](https://docs.cometbft.com/main/spec/rpc/)
* [Cosmos SDK Documentation](https://docs.cosmos.network/)
