---
description: >-
  Example code for the block JSON_RPC method. Complete guide on how to use block
  JSON_RPC in GetBlock Web3 documentation.
---

# block - Cardano

This endpoint returns a block and its transactions, identified by index or hash. Balance-changing operations are expressed in the Rosetta operation model.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type   | Required | Description                                |
| ------------------- | ------ | -------- | ------------------------------------------ |
| network\_identifier | object | Yes      | The network to query                       |
| block\_identifier   | object | Yes      | Partial identifier with an index or a hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/block' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "block_identifier": {
        "index": 10453789
    }
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/block',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "block_identifier": {"index": 10453789}})
    }
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/block',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "block_identifier": {"index": 10453789}}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "block": {
        "block_identifier": {
            "index": 10453789,
            "hash": "6e9e89632bc5c72030d3a486647e889c48d63e4da0643191b13566ad816d2d57"
        },
        "parent_block_identifier": {
            "index": 10453788,
            "hash": "000...parent"
        },
        "timestamp": 1617180599000,
        "transactions": [
            {
                "transaction_identifier": {
                    "hash": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"
                },
                "operations": [
                    {
                        "operation_identifier": {
                            "index": 0
                        },
                        "type": "output",
                        "status": "success",
                        "account": {
                            "address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"
                        },
                        "amount": {
                            "value": "9407625",
                            "currency": {
                                "symbol": "ADA",
                                "decimals": 6
                            }
                        }
                    }
                ]
            }
        ]
    }
}
```

## Response Parameters

| Field                           | Type    | Description                                |
| ------------------------------- | ------- | ------------------------------------------ |
| block.block\_identifier         | object  | Index and hash of the block                |
| block.parent\_block\_identifier | object  | Index and hash of the parent block         |
| block.timestamp                 | integer | Block timestamp in milliseconds            |
| block.transactions              | array   | Transactions with their Rosetta operations |

## Use Cases

* **Block Explorers**: Render block contents as Rosetta operations
* **Chain Indexing**: Stream blocks into a Rosetta-based indexer
* **Reconciliation**: Derive balance changes from block operations
* **Exchange Integration**: Detect deposits by scanning block operations

## Error Handling

| Error Code | Message         | Description                                              |
| ---------- | --------------- | -------------------------------------------------------- |
| 1          | Invalid request | The request body is malformed or missing required fields |
| 4          | Block not found | The requested block does not exist on this node          |
| 5          | Internal error  | The node failed to answer the request                    |
