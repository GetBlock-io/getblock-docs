---
description: >-
  Yellowstone gRPC is a Solana Geyser plugin developed by Triton One that feeds
  your application a continuous, low-latency stream of on-chain data
---

# Overview

Solana applications often need **live**, **high-throughput** access to on-chain events. Solana gRPC plugin solves this core problem of real-time blockchain data access.

### What is Yellowstone gRPC?&#x20;

Yellowstone gRPC is the name given to the Dragon’s Mouth Geyser plugin’s gRPC interface in Triton One’s “Yellowstone” suite for Solana. It allows opening streams and subscribing to native Solana on-chain events, receiving every new update in real time, with millisecond-level latency.

By plugging directly into validators, it pushes new **blocks**, **transactions**, and **account** updates to your backend the moment they occur.&#x20;

### How Yellowstone gRPC Geyser works

The Geyser Plugin hooks into validator callbacks for ledger events and publishes those events to its own internal queues. A gRPC server then streams the queued events over the network to subscribed clients.&#x20;

#### **Supported data streams & subscriptions**

Geyser gRPC supports streaming the full range of common **Solana events**:

* **Account updates** (writes): Every time an account’s data changes, a notification is emitted.
* **Transactions**: Each transaction processed by the leader generates a stream event with all associated account changes.&#x20;
* **Ledger entries**: Low‑level entry/shred events (raw blocks of ledger data) can also be streamed.
* **Block notifications**: Clients can subscribe to be notified when a new block is completed.
* **Slot notifications**: New slot boundaries (leaders or votes) can trigger slot events.

Every update stream can include full transaction metadata, instruction details, and parsed logs  – essentially everything you’d see in a [`getTransaction`](../../api-reference/solana-sol/gettransaction-solana.md) or [`getProgramAccounts`](../../api-reference/solana-sol/getprogramaccounts-solana.md) call, but pushed in real time.

In addition to streaming methods, Dragon’s Mouth also exposes several **unary RPCs** via the same gRPC interface for quick queries about:

* The Slot;
* Block height;
* Latest blockhash;
* Valid blockhash.&#x20;

Together, this provides a way to both fetch state on demand and receive updates in real time.

***

### Yellowstone gRPC API features

* **Near-zero latency**: By streaming directly from leaders, Dragon’s Mouth delivers updates often hundreds of milliseconds faster than standard RPC/WebSocket APIs.
* **High throughput**: The plugin can handle millions of events per minute under load, built for Solana’s high transaction volume. Optional compression can be applied for even more efficiency.&#x20;
* **Built-in support for bi-directional streaming**: Keep-alives, ping/pong frames help maintain long-lived connections.
* **Comprehensive streaming**: Clients can monitor virtually anything: token mints, program interactions, votes, etc.&#x20;
* **Protobuf/binary encoding**: Each message arrives **parsed** and **typed**, not raw base64. Clients get structured fields (account diffs, token balance changes, parsed logs, etc.) instead of raw blobs.
* **Rich filtering**: You can apply filters (by account key, owner program, data patterns, commitment level, etc.) so only matching updates are streamed.

Overall, applications can keep pace with Solana’s peak TPS without data loss, receive only relevant updates, save bandwidth, and react faster.

***

### Solana Geyser gRPC plugin use cases

Solana gRPC streaming capabilities are crucial for **time-sensitive** applications, apps that need to **react the moment on-chain state changes** without manual refreshes.&#x20;

gRPC API ideal use cases include:

* High-frequency trading or arbitrage systems (e.g. MEV bots);&#x20;
* On-chain indexers & archives;&#x20;
* Live analytics;
* Real-time monitors for DEXes, NFTs, wallets, etc.;&#x20;
* Alerting & notification systems;&#x20;
* DeFi strategy engines;&#x20;
* ..and any app that needs push‑style updates.

{% hint style="info" %}
Note that gRPC is not supported in browsers, so Yellowstone is intended for backend services.
{% endhint %}

***

### Why use Yellowstone gRPC API?

Using Yellowstone gRPC for your Solana data means you get a **high-throughput**, **low-latency**, **bidirectional streaming** channel.&#x20;

Instead of polling REST endpoints every few seconds or using Solana’s WebSocket API (which typically only updates after a block finalizes), the gRPC interface allows tracking every new event down the wire as it happens. &#x20;

Overall, it removes much of the boilerplate: your backend code subscribes once, then simply reacts to incoming messages
