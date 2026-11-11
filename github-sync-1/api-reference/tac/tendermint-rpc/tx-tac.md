---
description: >-
  Example code for the tx JSON-RPC method. Complete guide on how to use tx
  JSON-RPC in GetBlock Web3 documentation.
---

# tx - TAC

Returns a transaction by its hash, with its execution result and (optionally) a Merkle inclusion proof.

## Method

```
tx
```

## Parameters

| Parameter | Type   | Description            |
| --------- | ------ | ---------------------- |
| hash      | string | Transaction hash       |
| prove     | string | Include a Merkle proof |

## Example

{% code overflow="wrap" %}
```bash
export TAC_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${TAC_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "tx", "params": {"hash": "0x9A1B2C...", "prove": "true"}}'

# URI over HTTP (GET)
curl "${TAC_TM}tx?hash=0x9A1B2C...&prove=true"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "hash": "9A1B2C...",
        "height": "5123456",
        "index": 0,
        "tx_result": {
            "code": 0,
            "gas_used": "42000",
            "events": []
        },
        "tx": "Cg8K..."
    }
}
```

## Response Fields

| Field      | Type   | Description                         |
| ---------- | ------ | ----------------------------------- |
| hash       | string | The transaction hash                |
| height     | string | Block height of inclusion           |
| tx\_result | object | Execution result: code, gas, events |
| tx         | string | Base64-encoded transaction bytes    |

## Use Cases

* **Receipt lookup**: Resolve a transaction and its result by hash
* **Proofs**: Request prove=true for a Merkle inclusion proof

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
