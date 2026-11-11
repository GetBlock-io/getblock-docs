---
description: >-
  Example code for the dump_consensus_state JSON-RPC method. Complete guide on
  how to use dump_consensus_state JSON-RPC in GetBlock Web3 documentation.
---

# dump\_consensus\_state - Cosmos

Returns the full consensus state: the node's round state plus the state of all peers.

## Method

```
dump_consensus_state
```

## Parameters

This method takes no parameters.

## Example

{% code overflow="wrap" %}
```bash
export COSMOS_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${COSMOS_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "dump_consensus_state", "params": {}}'

# URI over HTTP (GET)
curl "${COSMOS_TM}dump_consensus_state"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "round_state": {
            "height": "31378845"
        },
        "peers": []
    }
}
```

## Response Fields

| Field        | Type   | Description                 |
| ------------ | ------ | --------------------------- |
| round\_state | object | The node's full round state |
| peers        | array  | Per-peer consensus state    |

## Use Cases

* **Deep diagnostics**: Inspect consensus state across peers

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
