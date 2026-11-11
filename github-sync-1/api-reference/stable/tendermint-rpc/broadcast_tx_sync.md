---
description: >-
  Example code for the broadcast_tx_sync JSON-RPC method. Complete guide on how
  to use broadcast_tx_sync JSON-RPC in GetBlock Web3 documentation.
---

# broadcast\_tx\_sync - Stable

Broadcasts a transaction and returns after CheckTx (mempool validation), without waiting for the transaction to be included in a block.

## Method

```
broadcast_tx_sync
```

## Parameters

| Parameter | Type   | Description                       |
| --------- | ------ | --------------------------------- |
| tx        | string | Base64-encoded signed transaction |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${STABLE_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "broadcast_tx_sync", "params": {"tx": "Cg8KQ..."}}'

# URI over HTTP (GET)
curl "${STABLE_TM}broadcast_tx_sync?tx=Cg8KQ..."
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "code": 0,
        "data": "",
        "log": "",
        "hash": "9A1B2C..."
    }
}
```

## Response Fields

| Field | Type   | Description                                     |
| ----- | ------ | ----------------------------------------------- |
| code  | number | CheckTx result code (0 = accepted into mempool) |
| hash  | string | The transaction hash                            |
| log   | string | CheckTx log or error message                    |

## Use Cases

* **Submission**: Broadcast and confirm mempool acceptance

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
