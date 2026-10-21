---
description: >-
  Example code for the latest block JSON-RPC method. Complete guide on how to
  use latest block JSON-RPC in GetBlock Web3 documentation.
---

# /cosmos/base/tendermint/v1beta1/blocks/latest - TAC

Returns the latest block via the Cosmos base tendermint service, including its header and transactions.

## Endpoint

```http
GET /cosmos/base/tendermint/v1beta1/blocks/latest
```

## Example

{% code overflow="wrap" %}
```bash
export TAC_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TAC_REST}cosmos/base/tendermint/v1beta1/blocks/latest"
```
{% endcode %}

## Response

```json
{
    "block_id": {
        "hash": "A1B2..."
    },
    "block": {
        "header": {
            "height": "5123456",
            "chain_id": "tacchain_2390-1",
            "time": "2026-06-01T10:31:22Z"
        },
        "data": {
            "txs": []
        }
    }
}
```

## Response Fields

| Field          | Type   | Description                              |
| -------------- | ------ | ---------------------------------------- |
| block\_id      | object | The block's id and hash                  |
| block.header   | object | Block header: height, chain\_id, time    |
| block.data.txs | array  | Base64-encoded transactions in the block |

## Use Cases

* **Tip read**: Get the latest block over REST
* **Sync tracking**: Read the current height without Tendermint RPC

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
