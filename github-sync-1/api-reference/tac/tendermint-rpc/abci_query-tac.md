---
description: >-
  Example code for the abci_query JSON-RPC method. Complete guide on how to use
  abci_query JSON-RPC in GetBlock Web3 documentation.
---

# abci\_query - TAC

Runs an ABCI query against the application at a given path, optionally at a historical height and with a Merkle proof. This is the low-level transport the Cosmos SDK gRPC/REST layer uses to read module state.

## Method

```
abci_query
```

## Parameters

| Parameter | Type   | Description                                                  |
| --------- | ------ | ------------------------------------------------------------ |
| path      | string | ABCI query path, e.g. /cosmos.bank.v1beta1.Query/AllBalances |
| data      | string | Hex-encoded request bytes                                    |
| height    | string | Height to query (0 = latest)                                 |
| prove     | string | Include a Merkle proof                                       |

## Example

{% code overflow="wrap" %}
```bash
export TAC_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${TAC_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "abci_query", "params": {"path": "/cosmos.bank.v1beta1.Query/AllBalances", "data": "0x0a2b...", "height": "0", "prove": "false"}}'

# URI over HTTP (GET)
curl "${TAC_TM}abci_query?path=/cosmos.bank.v1beta1.Query/AllBalances&data=0x0a2b...&height=0&prove=false"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "response": {
            "code": 0,
            "value": "Cg0K...",
            "height": "5123456"
        }
    }
}
```

## Response Fields

| Field           | Type   | Description                                                            |
| --------------- | ------ | ---------------------------------------------------------------------- |
| response.code   | number | Query result code (0 = success)                                        |
| response.value  | string | Base64-encoded response bytes (decode with the module's protobuf type) |
| response.height | string | Height the query was run at                                            |

## Use Cases

* **Raw state reads**: Query any module's state by path
* **Proofs**: Request prove=true for a Merkle proof of the value

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
