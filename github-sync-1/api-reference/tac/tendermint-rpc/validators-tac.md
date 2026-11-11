---
description: >-
  Example code for the validators JSON-RPC method. Complete guide on how to use
  validators JSON-RPC in GetBlock Web3 documentation.
---

# validators - TAC

Returns the validator set at a given height, paginated. Omitting height returns the latest set.

## Method

```
validators
```

## Parameters

| Parameter | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| height    | string | Block height (omit for latest) |
| page      | string | Page number                    |
| per\_page | string | Results per page (max 100)     |

## Example

{% code overflow="wrap" %}
```bash
export TAC_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${TAC_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "validators", "params": {"height": "5123456", "page": "1", "per_page": "100"}}'

# URI over HTTP (GET)
curl "${TAC_TM}validators?height=5123456&page=1&per_page=100"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "block_height": "5123456",
        "validators": [
            {
                "address": "8BFAB70E...",
                "voting_power": "1000000",
                "proposer_priority": "0"
            }
        ],
        "count": "1",
        "total": "100"
    }
}
```

## Response Fields

| Field         | Type   | Description                                                  |
| ------------- | ------ | ------------------------------------------------------------ |
| block\_height | string | Height of the returned validator set                         |
| validators    | array  | Validators with address, voting power, and proposer priority |
| total         | string | Total validator count                                        |

## Use Cases

* **Validator set**: Read the active set at a height
* **Voting power**: Compute stake distribution for a block

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
