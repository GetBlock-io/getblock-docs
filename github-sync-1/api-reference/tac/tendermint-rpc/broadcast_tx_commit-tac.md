---
description: >-
  Example code for the broadcast_tx_commit JSON-RPC method. Complete guide on
  how to use broadcast_tx_commit JSON-RPC in GetBlock Web3 documentation.
---

# broadcast\_tx\_commit - TAC

Broadcasts a transaction and waits until it is committed in a block, returning both the CheckTx and the DeliverTx (execution) results. Use sparingly — it blocks until commit.

## Method

```
broadcast_tx_commit
```

## Parameters

| Parameter | Type   | Description                       |
| --------- | ------ | --------------------------------- |
| tx        | string | Base64-encoded signed transaction |

## Example

{% code overflow="wrap" %}
```bash
export TAC_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${TAC_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "broadcast_tx_commit", "params": {"tx": "Cg8KQ..."}}'

# URI over HTTP (GET)
curl "${TAC_TM}broadcast_tx_commit?tx=Cg8KQ..."
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "check_tx": {
            "code": 0
        },
        "tx_result": {
            "code": 0,
            "gas_used": "42000"
        },
        "height": "5123457",
        "hash": "9A1B2C..."
    }
}
```

## Response Fields

| Field      | Type   | Description                                  |
| ---------- | ------ | -------------------------------------------- |
| check\_tx  | object | Mempool validation result                    |
| tx\_result | object | Execution (DeliverTx) result after inclusion |
| height     | string | Block height of inclusion                    |
| hash       | string | The transaction hash                         |

## Use Cases

* **Confirmed submission**: Broadcast and wait for the committed result
* **Scripts**: Simplify flows that need the final result inline

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
