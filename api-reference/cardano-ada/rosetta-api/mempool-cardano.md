---
description: >-
  Example code for the mempool JSON-RPC method. Complete guide on how to use
  mempool JSON-RPC in GetBlock Web3 documentation.
---

# mempool - Cardano

This endpoint returns the transaction identifiers currently in the node's mempool.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type   | Required | Description              |
| ------------------- | ------ | -------- | ------------------------ |
| network\_identifier | object | Yes      | The network to query     |
| metadata            | object | No       | Optional metadata object |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/mempool' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "metadata": {}
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/mempool',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "metadata": {}})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/mempool',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "metadata": {}}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "transaction_identifiers": [
        {
            "hash": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"
        }
    ]
}
```

## Response Parameters

| Field                    | Type  | Description                                     |
| ------------------------ | ----- | ----------------------------------------------- |
| transaction\_identifiers | array | Hashes of transactions currently in the mempool |

## Use Cases

* **Pending Monitoring**: List unconfirmed transactions
* **Payment Detection**: Detect an incoming transaction before confirmation
* **Congestion Signals**: Gauge mempool depth
* **Status Tracking**: Confirm a submitted transaction is pending

## Error Handling

| Error Code | Message         | Description                                              |
| ---------- | --------------- | -------------------------------------------------------- |
| 1          | Invalid request | The request body is malformed or missing required fields |
| 4          | Block not found | The requested block does not exist on this node          |
| 5          | Internal error  | The node failed to answer the request                    |
