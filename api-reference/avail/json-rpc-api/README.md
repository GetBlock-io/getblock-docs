---
description: >-
  GetBlock provides fast and reliable access to Avail nodes via JSON-RPC API.
  Connect to the Avail network without running your own infrastructure.
---

# JSON RPC API

Avail's Substrate JSON-RPC methods, organized by namespace. These are request/response calls available over both the JSON-RPC (HTTP) and WebSocket interfaces. For pub-sub subscriptions, see the [WebSocket Subscriptions](../websocket-subscriptions-api/) API.

## System

| Method                                                       | Description                             |
| ------------------------------------------------------------ | --------------------------------------- |
| [system\_chain](system_chain-avail.md)                       | Chain name                              |
| [system\_health](system_health-avail.md)                     | Node health and sync flags              |
| [system\_name](system_name-avail.md)                         | Client implementation name              |
| [system\_version](system_version-avail.md)                   | Client version                          |
| [system\_properties](system_properties-avail.md)             | Token symbol, decimals, and SS58 format |
| [system\_syncState](system_syncstate-avail.md)               | Sync progress heights                   |
| [system\_accountNextIndex](system_accountnextindex-avail.md) | Next nonce for an account               |
| [system\_chainType](system_chaintype-avail.md)               | Chain type: Live, Local, or Development |
| [system\_nodeRoles](system_noderoles-avail.md)               | Node roles                              |

## Chain

| Method                                                       | Description                     |
| ------------------------------------------------------------ | ------------------------------- |
| [chain\_getBlock](chain_getblock-avail.md)                   | Full block by hash, or latest   |
| [chain\_getBlockHash](chain_getblockhash-avail.md)           | Block hash by number, or latest |
| [chain\_getHeader](chain_getheader-avail.md)                 | Block header by hash, or latest |
| [chain\_getFinalizedHead](chain_getfinalizedhead-avail.md)   | Latest finalized block hash     |
| [chain\_getRuntimeVersion](chain_getruntimeversion-avail.md) | Runtime version at a block      |

## State

| Method                                                       | Description                          |
| ------------------------------------------------------------ | ------------------------------------ |
| [state\_getRuntimeVersion](state_getruntimeversion-avail.md) | Runtime version and APIs             |
| [state\_getMetadata](state_getmetadata-avail.md)             | Runtime metadata in SCALE hex        |
| [state\_getStorage](state_getstorage-avail.md)               | Raw storage value by key             |
| [state\_getKeysPaged](state_getkeyspaged-avail.md)           | Paged storage keys under a prefix    |
| [state\_queryStorageAt](state_querystorageat-avail.md)       | Values for multiple keys at a block  |
| [state\_getKeys](state_getkeys-avail.md)                     | Storage keys under a prefix, unpaged |
| [state\_getStorageHash](state_getstoragehash-avail.md)       | Hash of a storage value              |
| [state\_getReadProof](state_getreadproof-avail.md)           | Merkle proof for storage keys        |
| [state\_call](state_call-avail.md)                           | Invoke a runtime API                 |

## Childstate

| Method                                                   | Description                |
| -------------------------------------------------------- | -------------------------- |
| [childstate\_getStorage](childstate_getstorage-avail.md) | Value in a child trie      |
| `childstate_getStorageHash`                              | Hash of a child-trie value |

## Author

| Method                                                         | Description               |
| -------------------------------------------------------------- | ------------------------- |
| [author\_submitExtrinsic](author_submitextrinsic-avail.md)     | Submit a signed extrinsic |
| [author\_pendingExtrinsics](author_pendingextrinsics-avail.md) | Extrinsics in the pool    |

## Payment

| Method                                                       | Description                   |
| ------------------------------------------------------------ | ----------------------------- |
| [payment\_queryInfo](payment_queryinfo-avail.md)             | Estimate fee for an extrinsic |
| [payment\_queryFeeDetails](payment_queryfeedetails-avail.md) | Detailed fee breakdown        |

## New JSON-RPC (v1)

| Method                                                          | Description             |
| --------------------------------------------------------------- | ----------------------- |
| [chainSpec\_v1\_chainName](chainspec_v1_chainname-avail.md)     | Chain name              |
| [chainSpec\_v1\_genesisHash](chainspec_v1_genesishash-avail.md) | Genesis hash            |
| [chainSpec\_v1\_properties](chainspec_v1_properties-avail.md)   | Chain properties        |
| [transaction\_v1\_broadcast](transaction_v1_broadcast-avail.md) | Broadcast a transaction |

## RPC

| Method                               | Description                |
| ------------------------------------ | -------------------------- |
| [rpc\_methods](rpc_methods-avail.md) | List available RPC methods |

## Support

| Contact                                           |
| ------------------------------------------------- |
| [support@getblock.io](mailto:support@getblock.io) |
