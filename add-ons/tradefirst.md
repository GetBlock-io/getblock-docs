---
description: >-
  An infrastructure that combines other GetBlock and external infrastructures,
  specifically designed for high-frequency traders (HFT) on Solana.
---

# TradeFirst

TradeFirst is an infrastructure that combines other GetBlock and external infrastructures, specifically designed for high-frequency traders (HFT) on Solana. It combines multiple performance optimizations:&#x20;

* fast data streaming (StreamFirst)
* intelligent transaction routing (LandFirst)

into a single, integrated solution for professional trading operations.

_**Interested in building on Solana with TradeFirst?**_ [_**Reach us**_](https://getblock.io/contact) _**for more information.**_

#### Core Value Proposition

TradeFirst provides two-sided latency optimization for Solana trading:

1. Signal Detection Side: Faster awareness of on-chain events via StreamFirst (data streaming).
2. Execution Side: Faster transaction delivery and inclusion via Blazar, SubSlot, and LandFirst routing

By optimizing both sides, TradeFirst enables traders to see opportunities earlier and execute trades faster than competitors using standard infrastructure.

#### Core Technology Stack

1. **StreamFirst (Data Streaming)**
   1. Accelerated Yellowstone gRPC implementation
   2. Optimized shred-stream network access
   3. Fastest on-chain data delivery for signal detection
2. **LandFirst (Multi-Path Routing)**
   1. SWQoS priority connections
3. **Jito integration**&#x20;
   1. Geo routing
   2. Stake density topology
   3. Leadership scheduling

{% hint style="success" %}
Future Addition:

* Shred Stream Access (coming soon): Direct raw shred delivery for even earlier data access
{% endhint %}

### Technical Architecture

#### How TradeFirst Works

![](<../.gitbook/assets/unknown (4).png>)

Complete Trading Cycle:

1. Signal Detection: StreamFirst delivers on-chain updates 17ms faster than standard methods
2. Strategy Execution: Your trading logic analyzes data and generates orders
3. Transaction Submission: Blazar optimizes transaction structure and routing
4. Timing Control: SubSlot precisely times submission within the slot window
5. Path Selection: LandFirst routes via optimal path (SWQoS or Jito)
6. Block Inclusion: Transaction lands in the current or next slot with high probability

### Use cases

This is highly recommended for High-frequency trading (HFT) traders, which is specifically designed for:&#x20;

1\. **High-Frequency Trading Firms**

These are professional trading teams running large volumes of rapid-fire transactions on Solana, including cross-DEX arbitrage, statistical arbitrage across correlated token pairs, tight-spread market making, and fast liquidity rebalancing. For them, execution speed and reliability directly impact profitability, making this solution an essential part of their trading infrastructure.

**2. MEV Searchers**

MEV searchers rely on precise timing and high-priority execution for strategies like sandwich attacks, liquidation sniping, protocol arbitrage, or fast NFT flips. Since these opportunities exist for only seconds—and often compete with other searchers—having stronger execution reliability gives them a significant edge.

**3. Algorithmic Trading Operations**

Algorithmic trading systems continuously react to on-chain data, whether they're following momentum signals, trading based on oracle movements, optimizing yield across protocols, or executing automated rebalancing strategies. These systems need a consistent, low-latency execution layer to ensure their models perform as expected without disruptions.

**4. Proprietary Trading Desks**

Small teams and professional traders running their own capital depend on solid infrastructure without wanting to build it in-house. They benefit from flexible setups that support diverse strategies, predictable pricing so they know their cost structure, and reliable support for performance tuning or issue resolution.

**5. Institutional Crypto Traders**

Institutions executing high-volume Solana strategies require enterprise-level stability, compliance-friendly monitoring, volume-based pricing, and service-level guarantees. This solution gives them the dependable infrastructure and dedicated support needed to operate at scale while maintaining regulatory and operational standards.

***

For consultation on optimal deployment architecture for your specific use case, contact [GetBlock support team](https://getblock.io/contact/)

<br>
