---
description: >-
  Example code for the block/transaction JSON_RPC method. Complete guide on how
  to use block/transaction JSON_RPC in GetBlock Web3 documentation.
---

# block/transaction - Cardano

This endpoint returns a single transaction from a block, identified by the block and transaction identifiers. It is used when a block listing references a transaction that must be fetched separately.

## Parameters

The request body is a JSON object with the following fields.

| Field                   | Type   | Required | Description                               |
| ----------------------- | ------ | -------- | ----------------------------------------- |
| network\_identifier     | object | Yes      | The network to query                      |
| block\_identifier       | object | Yes      | Full block identifier with index and hash |
| transaction\_identifier | object | Yes      | The transaction hash to fetch             |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/block/transaction' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "block_identifier": {
        "index": 10453789,
        "hash": "6e9e89632bc5c72030d3a486647e889c48d63e4da0643191b13566ad816d2d57"
    },
    "transaction_identifier": {
        "hash": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"
    }
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/block/transaction',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "block_identifier": {"index": 10453789, "hash": "6e9e89632bc5c72030d3a486647e889c48d63e4da0643191b13566ad816d2d57"}, "transaction_identifier": {"hash": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"}})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/block/transaction',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "block_identifier": {"index": 10453789, "hash": "6e9e89632bc5c72030d3a486647e889c48d63e4da0643191b13566ad816d2d57"}, "transaction_identifier": {"hash": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"}}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "transaction": {
        "transaction_identifier": {
            "hash": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"
        },
        "operations": [
            {
                "operation_identifier": {
                    "index": 0
                },
                "type": "input",
                "status": "success",
                "account": {
                    "address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"
                },
                "amount": {
                    "value": "-9410563",
                    "currency": {
                        "symbol": "ADA",
                        "decimals": 6
                    }
                }
            }
        ]
    }
}
```

## Response Parameters

| Field                               | Type   | Description                          |
| ----------------------------------- | ------ | ------------------------------------ |
| transaction.transaction\_identifier | object | The transaction hash                 |
| transaction.operations              | array  | The transaction's Rosetta operations |

## Use Cases

* **Transaction Views**: Fetch full operation detail for one transaction
* **Deposit Confirmation**: Confirm a specific transaction's operations
* **Reconciliation**: Reconcile a single transaction's balance changes
* **Auditing**: Retrieve a transaction referenced by a block listing

## Error Handling

| Error Code | Message         | Description                                              |
| ---------- | --------------- | -------------------------------------------------------- |
| 1          | Invalid request | The request body is malformed or missing required fields |
| 4          | Block not found | The requested block does not exist on this node          |
| 5          | Internal error  | The node failed to answer the request                    |
