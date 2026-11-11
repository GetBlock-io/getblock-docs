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

{% hint style="info" %}
Flashblocks are simply a class of [RPC API methods](flashblocks-api/) that are now active on GetBlock endpoints for Base and Optimism. If you already run either chain with GetBlock, the new methods are available on your existing endpoint. If you are new, set up a Base or Optimism endpoint from the [GetBlock dashboard,](https://account.getblock.io) and the methods are there.
{% endhint %}

## Supported Networks

Flashblocks are documented per network. Endpoints, method tables, and chain-specific methods are covered on the dedicated pages:

* [**Base**](base-flashblocks.md)&#x20;
* [**Optimism**](optimism-flashblocks..md)
