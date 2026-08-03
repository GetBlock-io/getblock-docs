---
description: >-
  Learn what each GetBlock add-on does, the problem it solves, and which
  blockchain it serves
---

# Overview

An add-on extends your node with a capability that the standard JSON-RPC API does not provide: a second API layer, an address indexer, a bundler, a private mempool, or a real-time stream. Each add-on solves one specific problem that a standard node cannot solve on its own.

{% hint style="warning" %}
Every add-on in this section is **blockchain-specific**: it exists for one chain or one family of chains, and the configurator offers it only when your node runs a chain it supports. For services that work on every blockchain, see [Extra Services for Dedicated Nodes](../extra-services-for-dedicated-nodes/overview.md).
{% endhint %}

## The add-ons at a glance

| Add-on                                        | It solves                                                 | Blockchain                 |
| --------------------------------------------- | --------------------------------------------------------- | -------------------------- |
| [Beacon API](beacon-api.md)                   | The `eth_*` API cannot see validators, slots, or finality | Ethereum                   |
| [Blockbook](blockbook.md)                     | A plain node cannot answer address or wallet queries      | UTXO chains (Bitcoin-type) |
| [ERC-4337](eip-4337.md)                       | Standard clients do not serve account-abstraction methods | EVM chains                 |
| [Overlay Methods](overlay-methods.md)         | Data a contract never emitted does not exist on-chain     | EVM chains                 |
| [MEV Protection](mev-protection.md)           | Bots see your transactions in the public mempool          | EVM chains                 |
| [Yellowstone gRPC API](yellowstone-grpc-api/) | Standard RPC and WebSockets report Solana events too late | Solana                     |

### What each add-on gives you

#### 1. [Beacon API](beacon-api.md)&#x20;

Since the Merge, an Ethereum node runs as two coupled clients: the execution layer, which processes transactions and serves the familiar `eth_*` methods, and the consensus layer, which runs Proof-of-Stake. A standard node only exposes the execution layer. The Beacon API add-on opens the consensus layer alongside it, so a single dedicated node answers both.

#### 2. [Blockbook](blockbook.md) — UTXO chains

A Bitcoin-type node tracks unspent transaction outputs, but it does not organize them by address — ask a plain node for the balance of an address and it has no direct way to reply. Blockbook, an indexer built by Trezor, maintains an address-indexed and xpub-indexed view on top of the chain and answers those questions from its index.

#### 3. [ERC-4337](eip-4337.md) — EVM chains

Account abstraction lets a smart contract act as a wallet — enabling gasless transactions, third-party gas sponsorship, and custom validation logic — but standard clients such as `geth` and `reth` do not understand UserOperations, so they cannot serve the ERC-4337 methods. This add-on runs a bundler as a sidecar next to your node: it validates UserOperations, holds them in its own mempool, and settles them on-chain through the EntryPoint contract

#### 4. [Overlay Methods](overlay-methods.md) — EVM chains

If a contract shipped without an event you now need — a missing `Transfer` log, an unindexed state change — that data does not exist on-chain, and re-deploying a fixed contract cannot rewrite the past. Overlay methods take the real historical state and replay it through your version of the contract: the chain's inputs stay real, only the contract logic changes, and out come the logs the original contract would have emitted.

{% hint style="info" %}
Overlay Methods is not a plugin on top of a standard node. It is a separate custom `reth` build (Oregon) that replaces the standard dedicated node. The catalog lists it as an add-on for consistency.
{% endhint %}

#### 5. [MEV Protection](mev-protection.md) — EVM chains

A pending transaction normally waits in the public mempool, where MEV bots watch for trades they can exploit — front-running your trade, sandwiching it between two of their own, or back-running the price gap it opens. Every one of those attacks depends on the bot seeing your transaction while it waits. MEV Protection routes your transactions through a private mempool, in partnership with Merkle, so a searcher never sees them and has nothing to attack.

#### 6. [Yellowstone gRPC API](yellowstone-grpc-api/) — Solana

Yellowstone gRPC is a high-performance Solana Geyser plugin, built by Triton One, that streams on-chain data directly from validators with millisecond-level latency—often hundreds of milliseconds faster than standard RPC or WebSocket APIs —and can handle millions of events per minute.

### How to enable an add-on

In the dedicated node configurator, Step 3 (**Select API and Add-ons**) lists the add-ons available for your chain — check the ones you need. For a node that is already running, open the node's **Add-ons** tab in the Dashboard: from there you can add a new add-on, cancel an active one, or create an endpoint for it. Included add-ons come at no extra cost depending on your configuration; advanced add-ons are billed in addition to the base node price.
