---
description: >-
  Example code for the ledger info JSON-RPC method. Complete guide on how to use
  ledger info JSON-RPC in GetBlock Web3 documentation.
---

# ledger info - Movement

Returns the current ledger information for the node: chain ID, current epoch, ledger version, oldest retained version, and current block height. The entry point for confirming which network a node serves and how far it has synced.

## Endpoint

```http
GET /v1
```

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1"
```
{% endcode %}

## Response

```json
{
    "chain_id": 126,
    "epoch": "742",
    "ledger_version": "285000000",
    "oldest_ledger_version": "0",
    "ledger_timestamp": "1736772000000000",
    "node_role": "full_node",
    "oldest_block_height": "0",
    "block_height": "120000000",
    "git_hash": "abcd123"
}
```

## Response Fields

| Field           | Type    | Description                                 |
| --------------- | ------- | ------------------------------------------- |
| chain\_id       | integer | Chain identifier (126 for Movement mainnet) |
| ledger\_version | string  | Current ledger version (transaction count)  |
| block\_height   | string  | Current block height                        |
| epoch           | string  | Current epoch                               |

## Use Cases

* **Network Confirmation**: Verify the chain ID and sync height
* **Head Tracking**: Read the current ledger version and block height

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
