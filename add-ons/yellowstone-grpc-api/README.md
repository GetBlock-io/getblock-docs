---
description: >-
  Power your Solana dApps and backends with the fastest, most reliable streaming
  data available. Yellowstone gRPC add-on is for apps that need every live event
  as fast as the network can deliver.
---

# Yellowstone gRPC API

## Overview

**Yellowstone gRPC** is a high-performance Solana Geyser plugin that provides real-time streaming access to on-chain data. Built by Triton One, it delivers blocks, transactions, and account updates with millisecond-level latency directly from Solana validators.

GetBlock offers **managed Yellowstone gRPC endpoints** as an add-on to Dedicated Solana Node subscriptions, eliminating the need for infrastructure setup and maintenance.

***

## Key Features

* **Near-zero latency**: Streams data directly from validators, often hundreds of milliseconds faster than standard RPC/WebSocket APIs
* **High throughput**: Handles millions of events per minute under load
* **Comprehensive streaming**: Monitor accounts, transactions, blocks, slots, and program interactions in real-time
* **Rich filtering**: Subscribe only to relevant updates using account keys, owner programs, or commitment levels
* **Protobuf encoding**: Receive parsed, typed messages instead of raw base64 data
* **Bidirectional streaming**: Maintain long-lived connections with built-in keep-alives

***

## Supported Data Streams

Yellowstone gRPC supports streaming the full range of Solana events:

| Stream Type      | Description                                                      |
| ---------------- | ---------------------------------------------------------------- |
| **Accounts**     | Real-time account updates including lamports, owner, and data    |
| **Transactions** | Full transaction data with metadata and instruction details      |
| **Blocks**       | Block metadata including slot, parent slot, and timestamp        |
| **Slots**        | Slot notifications as they're processed by the leader            |
| **Block Meta**   | Block metadata with transaction counts and execution status      |

***

## Use Cases

Yellowstone gRPC is ideal for time-sensitive applications that need to react instantly to on-chain state changes:

* High-frequency trading and MEV bots
* On-chain indexers and data archives
* Real-time analytics dashboards
* DEX monitors and price feeds
* DeFi strategy engines
* Alerting and notification systems
* NFT marketplace trackers
* Wallet activity monitors

{% hint style="info" %}
**Note**: gRPC is not supported in browsers. Yellowstone is designed for backend services and server-side applications.
{% endhint %}

***

## Getting Started

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><mark style="color:blue;"><strong>Overview</strong></mark></td><td>Learn how Yellowstone gRPC works and why it's essential for real-time Solana applications</td><td><a href="overview.md">overview.md</a></td></tr><tr><td><mark style="color:blue;"><strong>Quickstart Guide</strong></mark></td><td>Enable the add-on and start streaming data with code examples in TypeScript, Python, Go, and Rust</td><td><a href="quickstart-guide.md">quickstart-guide.md</a></td></tr></tbody></table>

***

## Prerequisites

To use Yellowstone gRPC on GetBlock:

* A GetBlock account (sign up at [getblock.io](https://getblock.io))
* A [Dedicated Solana Node](https://docs.getblock.io/getting-started/plans-and-limits/choosing-your-plan) subscription
* Yellowstone gRPC add-on enabled (included at no extra cost with Dedicated Nodes)

***

## Quick Example

Here's a minimal TypeScript example to start streaming account updates:

```typescript
import Client, {
  SubscribeRequest,
  CommitmentLevel
} from "@triton-one/yellowstone-grpc";

const client = new Client("https://go.getblock.io/", "YOUR_ACCESS_TOKEN");
const stream = await client.subscribe();

stream.write({
  accounts: ["YourWalletPubkeyHere"],
  commitment: CommitmentLevel.PROCESSED,
} as SubscribeRequest);

stream.on("data", (msg) => {
  if (msg.accountChange) {
    console.log(`Account updated: ${msg.accountChange.pubkey}`);
  }
});
```

***

## Additional Resources

* [Yellowstone gRPC GitHub Repository](https://github.com/rpcpool/yellowstone-grpc)
* [GetBlock Solana Nodes](https://getblock.io/nodes/sol/)
* [GetBlock Support](https://getblock.io/contact/)

***

## Need Help?

Our support team is available 24/7 to assist with:

* Add-on activation and endpoint setup
* Integration guidance and troubleshooting
* Performance optimization
* Custom solutions for enterprise needs

Contact us through the [GetBlock dashboard](https://getblock.io) or visit our [support page](https://getblock.io/contact/).

