---
tags:
  - kusama
---

# Substrate JSON RPC API - Kusama

Kusama's Substrate JSON-RPC methods, organized by namespace. These are request/response calls available over both the JSON-RPC (HTTP) and WebSocket interfaces. For pub-sub subscriptions, see the [WebSocket Subscriptions](../websocket-subscriptions-api-kusama/) API.

## Available methods

### System

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td><code>system_chain</code></td><td>Chain name</td></tr><tr><td><code>system_health</code></td><td>Node health and sync flags</td></tr><tr><td><code>system_name</code></td><td>Client implementation name</td></tr><tr><td><code>system_version</code></td><td>Client version</td></tr><tr><td><code>system_properties</code></td><td>Token symbol, decimals, SS58 format</td></tr><tr><td><code>system_syncState</code></td><td>Sync progress heights</td></tr><tr><td><code>system_accountNextIndex</code></td><td>Next nonce for an account</td></tr></tbody></table>

### Chain

| Method                   | Description                      |
| ------------------------ | -------------------------------- |
| `chain_getBlock`         | Full block by hash (or latest)   |
| `chain_getBlockHash`     | Block hash by number (or latest) |
| `chain_getHeader`        | Block header by hash (or latest) |
| `chain_getFinalizedHead` | Latest finalized block hash      |

### State

| Method                    | Description                         |
| ------------------------- | ----------------------------------- |
| `state_getRuntimeVersion` | Runtime version and APIs            |
| `state_getMetadata`       | Runtime metadata (SCALE hex)        |
| `state_getStorage`        | Raw storage value by key            |
| `state_getKeysPaged`      | Paged storage keys under a prefix   |
| `state_queryStorageAt`    | Values for multiple keys at a block |

### Author

| Method                     | Description               |
| -------------------------- | ------------------------- |
| `author_submitExtrinsic`   | Submit a signed extrinsic |
| `author_pendingExtrinsics` | Extrinsics in the pool    |

### Payment

| Method                    | Description                   |
| ------------------------- | ----------------------------- |
| `payment_queryInfo`       | Estimate fee for an extrinsic |
| `payment_queryFeeDetails` | Detailed fee breakdown        |

### RPC

| Method        | Description                |
| ------------- | -------------------------- |
| `rpc_methods` | List available RPC methods |

## Support

Support: [support@getblock.io](mailto:support@getblock.io)
