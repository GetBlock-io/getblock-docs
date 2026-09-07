---
description: >-
  JSON-RPC API reference for Kusama's Substrate node. Explore method details,
  request examples, and GetBlock endpoint integration.
---

# Substrate JSON\_RPC API

Kusama's Substrate JSON-RPC methods, organized by namespace. These are request/response calls available over both the JSON-RPC (HTTP) and WebSocket interfaces. For pub-sub subscriptions, see the [WebSocket Subscriptions API](../websocket-subscriptions-api/).

## System

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>system_chain</td><td>Chain name</td></tr><tr><td>system_health</td><td>Node health and sync flags</td></tr><tr><td>system_name</td><td>Client implementation name</td></tr><tr><td>system_version</td><td>Client version</td></tr><tr><td>system_properties</td><td>Token symbol, decimals, SS58 format</td></tr><tr><td>system_syncState</td><td>Sync progress heights</td></tr><tr><td>system_accountNextIndex</td><td>Next nonce for an account</td></tr><tr><td>system_chainType</td><td>Chain type (Live/Local/Development)</td></tr><tr><td>system_nodeRoles</td><td>Node roles</td></tr></tbody></table>

## Chain

| Method                   | Description                      |
| ------------------------ | -------------------------------- |
| chain\_getBlock          | Full block by hash (or latest)   |
| chain\_getBlockHash      | Block hash by number (or latest) |
| chain\_getHeader         | Block header by hash (or latest) |
| chain\_getFinalizedHead  | Latest finalized block hash      |
| chain\_getRuntimeVersion | Runtime version at a block       |

## State

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>state_getRuntimeVersion</td><td>Runtime version and APIs</td></tr><tr><td>state_getMetadata</td><td>Runtime metadata (SCALE hex)</td></tr><tr><td>state_getStorage</td><td>Raw storage value by key</td></tr><tr><td>state_getKeysPaged</td><td>Paged storage keys under a prefix</td></tr><tr><td>state_queryStorageAt</td><td>Values for multiple keys at a block</td></tr><tr><td>state_getKeys</td><td>Storage keys under a prefix (unpaged)</td></tr><tr><td>state_getStorageHash</td><td>Hash of a storage value</td></tr><tr><td>state_getReadProof</td><td>Merkle proof for storage keys</td></tr><tr><td>state_call</td><td>Invoke a runtime API</td></tr></tbody></table>

## Childstate

| Method                     | Description                |
| -------------------------- | -------------------------- |
| childstate\_getStorage     | Value in a child trie      |
| childstate\_getStorageHash | Hash of a child-trie value |

## Author

| Method                    | Description               |
| ------------------------- | ------------------------- |
| author\_submitExtrinsic   | Submit a signed extrinsic |
| author\_pendingExtrinsics | Extrinsics in the pool    |

## Payment

| Method                   | Description                   |
| ------------------------ | ----------------------------- |
| payment\_queryInfo       | Estimate fee for an extrinsic |
| payment\_queryFeeDetails | Detailed fee breakdown        |

## New JSON-RPC (v1)

| Method                     | Description                        |
| -------------------------- | ---------------------------------- |
| chainSpec\_v1\_chainName   | Chain name (new spec)              |
| chainSpec\_v1\_genesisHash | Genesis hash (new spec)            |
| chainSpec\_v1\_properties  | Chain properties (new spec)        |
| transaction\_v1\_broadcast | Broadcast a transaction (new spec) |

## RPC

| Method       | Description                |
| ------------ | -------------------------- |
| rpc\_methods | List available RPC methods |
