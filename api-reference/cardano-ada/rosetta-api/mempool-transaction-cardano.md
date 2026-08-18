---
description: >-
  Example code for the mempool/transaction JSON-RPC method. Complete guide on
  how to use mempool/transactionJSON-RPC in GetBlock Web3 documentation.
---

# mempool/transaction - Cardano

This endpoint returns a single unconfirmed transaction from the mempool, expressed in the Rosetta operation model.

## Parameters

The request body is a JSON object with the following fields.

| Field                   | Type   | Required | Description                           |
| ----------------------- | ------ | -------- | ------------------------------------- |
| network\_identifier     | object | Yes      | The network to query                  |
| transaction\_identifier | object | Yes      | The mempool transaction hash to fetch |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/mempool/transaction' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/mempool/transaction',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "transaction_identifier": {"hash": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"}})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/mempool/transaction',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "transaction_identifier": {"hash": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"}}
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
}
```

## Response Parameters

| Field                               | Type   | Description                          |
| ----------------------------------- | ------ | ------------------------------------ |
| transaction.transaction\_identifier | object | The transaction hash                 |
| transaction.operations              | array  | The pending transaction's operations |

## Use Cases

* **Pending Detail**: Inspect an unconfirmed transaction's operations
* **Payment Preview**: Show incoming amounts before confirmation
* **Fee Analysis**: Read a pending transaction's operations for fees
* **Monitoring**: Track a specific pending transaction

## Error Handling

| Error Code | Message         | Description                                              |
| ---------- | --------------- | -------------------------------------------------------- |
| 1          | Invalid request | The request body is malformed or missing required fields |
| 4          | Block not found | The requested block does not exist on this node          |
| 5          | Internal error  | The node failed to answer the request                    |
