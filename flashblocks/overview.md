---
description: >-
  GetBlock exposes Flashblocks' data on supported OP Stack networks through the
  standard JSON-RPC and WebSocket interfaces.
---

# Overview

Flashblocks are partial blocks streamed by the sequencer as it builds. The mechanism is powered by Rollup-Boost, a sequencer sidecar built by Flashbots in collaboration with OP Labs. A single OP Stack block is divided into ten Flashblocks streamed at 200ms intervals, each one a delta containing the new transactions and resulting state changes since the previous Flashblock. The preconfirmed state is surfaced through the standard Ethereum JSON-RPC interface using the `pending` block tag, so existing tooling reads Flashblock state without modification.

## How Flashblocks Work

A standard OP Stack block is produced every 2 seconds. Flashblocks subdivide that interval into ten increments:

* The sequencer publishes a new Flashblock every \~200ms (`index` 0 through 9 within each parent block).
* `index` 0 carries the base block context; subsequent Flashblocks carry only the diff of new transactions and state changes.
* Reading any supported method against the `pending` tag returns the accumulated preconfirmed state from the latest Flashblock.
* The same call against `latest` returns the most recent fully sealed block.

Preconfirmations are strong signals, not guarantees. A streamed Flashblock can be excluded from the final block (a Flashblock reorg). The reorg rate is below 0.1%, but applications handling critical operations should confirm against finalized block data.

## How to Use Flashblocks

There are two access patterns:

* **`pending` tag (request/response)**: Pass `"pending"` as the block parameter to any supported read method to get the latest preconfirmed value over HTTP. This is the recommended approach for most applications, as it provides stable behavior with automatic fallback to standard blocks if Flashblocks become unavailable.
* **WebSocket streaming (subscriptions)**: Open a WebSocket connection and subscribe to a Flashblocks topic to receive each Flashblock as it is produced — five times per second. This is intended for latency-sensitive consumers such as trading systems. Applications should avoid a hard dependency on the WebSocket stream and treat it as an optimization layer over the RPC interface.

## Available Methods

Flashblocks reuse the standard Ethereum JSON-RPC surface. The following methods return preconfirmed Flashblock state when queried with the `pending` block tag:

| Method                    | Flashblock Behavior                                           |
| ------------------------- | ------------------------------------------------------------- |
| `eth_getBlockByNumber`    | Returns the current Flashblock when called with `pending`     |
| `eth_getBalance`          | Returns the balance reflecting the latest Flashblock state    |
| `eth_getTransactionCount` | Returns a nonce that accounts for transactions in Flashblocks |
| `eth_call`                | Executes against preconfirmed Flashblock state                |
| `eth_estimateGas`         | Estimates gas against preconfirmed Flashblock state           |
| `eth_getCode`             | Returns contract code at the preconfirmed state               |
| `eth_getStorageAt`        | Returns storage values at the preconfirmed state              |

The following methods reflect Flashblock state directly, without a block tag:

| Method                      | Flashblock Behavior                                              |
| --------------------------- | ---------------------------------------------------------------- |
| `eth_getTransactionReceipt` | Returns a receipt as soon as a transaction lands in a Flashblock |
| `eth_getTransactionByHash`  | Returns transaction data for preconfirmed transactions           |

#### Dedicated Flashblocks methods:

| Method            | Description                                                            |
| ----------------- | ---------------------------------------------------------------------- |
| `eth_simulateV1`  | Simulates calls and transactions against the latest preconfirmed state |
| `eth_subscribe`   | Opens a WebSocket subscription to a Flashblocks stream                 |
| `eth_unsubscribe` | Cancels an active Flashblocks subscription                             |

#### WebSocket subscription types (passed to `eth_subscribe`):

| Subscription                | Description                                                                |
| --------------------------- | -------------------------------------------------------------------------- |
| `newFlashblocks`            | Full block state updates as each Flashblock is built                       |
| `newFlashblockTransactions` | Each transaction as it becomes preconfirmed into a Flashblock              |
| `pendingLogs`               | Contract event logs from preconfirmed transactions, with sub-block latency |

## Networks

Flashblocks are documented per network. Endpoints, method tables, and chain-specific methods are covered on the dedicated pages:

* **Base**&#x20;
* **Optimism**
