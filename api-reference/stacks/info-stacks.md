---
description: >-
  Example code for the info REST method. Complete guide on how to use info REST
  method in GetBlock Web3 documentation.
---

# info - Stacks

Returns core node information: peer and server version, network ID, the current burnchain (Bitcoin) block height, the Stacks chain tip height and hash, tenure height, and sync status.

## Endpoint

```http
GET /v2/info
```

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${STACKS_RPC}v2/info"
```
{% endcode %}

## Response

```json
{
    "peer_version": 4207599114,
    "burn_block_height": 867890,
    "stable_burn_block_height": 867889,
    "server_version": "stacks-node 3.1.0.0.8 (release/3.1.0.0.8)",
    "network_id": 1,
    "parent_network_id": 3652501241,
    "stacks_tip_height": 512345,
    "stacks_tip": "b1807a2d5f70910013af2c9e4b7a1d6f3028e5c7b9a0d4f1e6c3b8a2d5f70910",
    "stacks_tip_consensus_hash": "3a5b6e8f0c2d4a6b8e0f1c3d5a7b9e0f27c3b8a2",
    "tenure_height": 512345,
    "is_fully_synced": true
}
```

## Response Fields

| Field               | Type    | Description                              |
| ------------------- | ------- | ---------------------------------------- |
| peer\_version       | number  | P2P protocol version                     |
| burn\_block\_height | number  | Current Bitcoin (burnchain) block height |
| server\_version     | string  | Node software version string             |
| network\_id         | number  | Chain network ID (1 = mainnet)           |
| stacks\_tip\_height | number  | Height of the Stacks chain tip           |
| stacks\_tip         | string  | Block hash of the Stacks chain tip       |
| tenure\_height      | number  | Current Nakamoto tenure height           |
| is\_fully\_synced   | boolean | Whether the node is fully synced         |

## Use Cases

* **Health probe**: Confirm the node is up and fully synced
* **Sync tracking**: Compare stacks\_tip\_height against your indexer

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
