---
description: >-
  Example code for the consensus_state JSON-RPC method. Complete guide on how to
  use consensus_state JSON-RPC in GetBlock Web3 documentation.
---

# consensus\_state - TAC

Returns the current consensus state (round, step, and vote state) of the node.

## Method

```
consensus_state
```

## Parameters

This method takes no parameters.

## Example

{% code overflow="wrap" %}
```bash
export TAC_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${TAC_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "consensus_state", "params": {}}'

# URI over HTTP (GET)
curl "${TAC_TM}consensus_state"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "round_state": {
            "height/round/step": "5123457/0/1",
            "proposal_block_hash": ""
        }
    }
}
```

## Response Fields

| Field        | Type   | Description                                                  |
| ------------ | ------ | ------------------------------------------------------------ |
| round\_state | object | The node's current height/round/step and proposal/vote state |

## Use Cases

* **Consensus debugging**: Inspect the live round state

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
