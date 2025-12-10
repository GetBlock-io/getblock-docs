---
description: >-
  An infrastructure for delivering the fastest on-chain updates by combining
  software-level acceleration (Fast Yellowstone) with optimized network traffic
  and routing (via shred-stream).
---

# StreamFirst

StreamFirst is GetBlock's infrastructure solution for delivering faster (ultra-low latency) on-chain state updates from the Solana blockchain.&#x20;

By combining software-level acceleration with network-level optimization, StreamFirst enables developers to receive blockchain data faster than traditional RPC methods. This reduces the consistent polling updates, which either lead to timeouts or rate-limiting issues.

_**Interested in building on Solana with StreamFirst?**_ [_**Reach us**_](https://getblock.io/contact) _**for more information.**_

### Core Stack

StreamFirst consists of two core optimization layers:

1. Software-level acceleration: Accelerated Yellowstone gRPC implementation
2. Network-level acceleration: Optimized shred-stream delivery via direct validator connections.

StreamFirst optimizes the entire pipeline from validator broadcast to application delivery:

* The network layer ensures the earliest possible data reception
* Software layer ensures minimal processing latency
* Result: Fastest end-to-end delivery of on-chain state updates

### How It Works

#### Accelerated Yellowstone gRPC

Yellowstone is a high-performance Geyser plugin that streams real-time blockchain data via gRPC interfaces.  As a result of our partnership with the core Yellowstone team members, GetBlock's Accelerated Yellowstone implementation includes:

* **Optimized data serialization:** Optimized the original version of the low-level logic to improve the speed of state updates compared to the original Yellowstone implementation, and reduced overhead in encoding and transmitting blockchain state.
* **Enhanced filtering mechanisms:** More efficient subscription management for accounts, transactions, slots, and blocks
* **Improved connection handling:** Better resource management for sustained high-throughput streams

#### Shred-Stream Network Optimization

Solana validators propagate blocks by breaking them into small fragments called "shreds" and distributing them via the Turbine protocol. StreamFirst taps into this raw data stream:

* **Direct shred reception:** Receives block fragments (shreds) via UDP as validators broadcast them.&#x20;
* **Early state reconstruction:** Rebuilds block data before it's fully confirmed and distributed via standard RPC

#### Frankfurt Data Center - Europe's Solana Hub

GetBlock operates as a top-tier Solana node provider in Frankfurt, a zone with the highest density of Solana validator stake. This strategic positioning provides:

* Proximity to major validators: Direct access to high-stake validators concentrated in the region
* Ultra-low network latency: 6ms latency within Europe

![](<../.gitbook/assets/unknown (3).png>)

* Optimal shred reception: Positioned to receive validator broadcasts with minimal delay

The combination means you get blockchain state updates as validators are producing blocks, not after they've been fully processed and distributed.

### Technical Architecture

Data Flow:

1. Block Production: Solana validators produce blocks and fragment them into "shreds."
2. State Reconstruction: Shreds are decoded and reassembled into transactions and account updates
3. Accelerated Processing: Optimized Yellowstone plugin processes data with reduced overhead
4. Client Delivery: Structured blockchain data streams to your application via standard gRPC

### Why StreamFirst is Important for Developers

#### Speed Advantages

Traditional RPC methods introduce latency at multiple stages:

* Waiting for block confirmation
* HTTP request/response overhead
* JSON parsing and serialization

StreamFirst bypasses these bottlenecks by:

* Receiving data at the validator propagation speed
* Using binary gRPC protocol (faster than JSON)
* Streaming continuously without polling

{% hint style="success" %}
**Typical latency improvements:** 25-30ms faster than standard Yellowstone gRPC&#x20;
{% endhint %}

#### Use Cases

StreamFirst is ideal for applications where milliseconds matter:

* High-frequency trading bots: React to price changes before slower competitors
* MEV searchers: Identify arbitrage opportunities in real-time
* DeFi protocols: Monitor liquidation events and oracle updates instantly
* NFT sniping tools: Detect new listings and mints before they propagate widely
* Analytics platforms: Collect comprehensive on-chain data with minimal delay
* Wallet applications: Show users' balance and transaction updates in real-time

#### Data Types Available

StreamFirst supports all standard Yellowstone gRPC subscription types:

* Account updates: Monitor balance changes, data modifications, and ownership transfers
* Transaction streams: Receive all transactions or filter by program/account
* Slot updates: Track block production and commitment levels
* Block data: Full block information, including all transactions and metadata
* Program logs: Subscribe to specific program execution logs

### Deployment and Availability

StreamFirst is available as an add-on for GetBlock's dedicated Solana nodes. It's configured as a plugin in the node configurator and requires:

* Dedicated node subscription
* Geographic proximity to GetBlock's validator network for optimal performance
* Additional fee based on data throughput requirements

### Conclusion

StreamFirst gives developers a competitive edge by delivering Solana blockchain data at near-validator speed. The combination of accelerated Yellowstone software and optimized shard-stream networking, optimized shred-stream networking, and strategic Frankfurt positioning ensures low latency between your customer and dedicated node. This ensures your dApp has the earliest possible view of on-chain activity—critical for time-sensitive operations in DeFi, trading, and real-time analytics.

***

For consultation on optimal deployment architecture for your specific use case, contact [GetBlock support team](https://getblock.io/contact/)

<br>
