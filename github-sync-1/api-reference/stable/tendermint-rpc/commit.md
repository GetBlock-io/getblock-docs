---
description: >-
  Example code for the commit JSON-RPC method. Complete guide on how to use
  commit JSON-RPC in GetBlock Web3 documentation.
---

# commit - Stable

Returns the commit (validator signatures) for a block at a given height, with a flag indicating whether it is canonical.

## Method

```
commit
```

## Parameters

| Parameter | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| height    | string | Block height (omit for latest) |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${STABLE_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "commit", "params": {"height": "31378844"}}'

# URI over HTTP (GET)
curl "${STABLE_TM}commit?height=31378844"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "signed_header": {
            "header": {
                "height": "31378844"
            },
            "commit": {
                "height": "31378844",
                "round": 0,
                "signatures": []
            }
        },
        "canonical": true
    }
}
```

## Response Fields

| Field          | Type    | Description                                 |
| -------------- | ------- | ------------------------------------------- |
| signed\_header | object  | The block header plus its commit signatures |
| canonical      | boolean | Whether the commit is canonical             |

## Use Cases

* **Light-client verification**: Read the commit to verify a header

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
