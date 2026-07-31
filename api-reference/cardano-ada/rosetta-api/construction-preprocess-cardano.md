---
description: >-
  Example code for the construction/preprocess JSON-RPC method. Complete guide
  on how to use construction/preprocess JSON-RPC in GetBlock Web3 documentation.
---

# construction/preprocess - Cardano

This endpoint creates a request to fetch the metadata needed to construct a transaction, from the intended operations. It is the first online-preparation step of the construction flow.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type   | Required | Description                                                   |
| ------------------- | ------ | -------- | ------------------------------------------------------------- |
| network\_identifier | object | Yes      | The network to construct for                                  |
| operations          | array  | Yes      | The intended operations describing the transaction            |
| metadata            | object | No       | Optional construction metadata, such as relative time to live |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/construction/preprocess' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "operations": [
        {
            "operation_identifier": {
                "index": 0
            },
            "type": "input",
            "account": {
                "address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"
            },
            "amount": {
                "value": "-9410563",
                "currency": {
                    "symbol": "ADA",
                    "decimals": 6
                }
            },
            "coin_change": {
                "coin_identifier": {
                    "identifier": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642:0"
                },
                "coin_action": "coin_spent"
            }
        }
    ],
    "metadata": {}
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/construction/preprocess',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "operations": [{"operation_identifier": {"index": 0}, "type": "input", "account": {"address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"}, "amount": {"value": "-9410563", "currency": {"symbol": "ADA", "decimals": 6}}, "coin_change": {"coin_identifier": {"identifier": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642:0"}, "coin_action": "coin_spent"}}], "metadata": {}})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/construction/preprocess',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "operations": [{"operation_identifier": {"index": 0}, "type": "input", "account": {"address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"}, "amount": {"value": "-9410563", "currency": {"symbol": "ADA", "decimals": 6}}, "coin_change": {"coin_identifier": {"identifier": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642:0"}, "coin_action": "coin_spent"}}], "metadata": {}}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "options": {
        "relative_ttl": 1000,
        "transaction_size": 246
    }
}
```

## Response Parameters

| Field                  | Type   | Description                                              |
| ---------------------- | ------ | -------------------------------------------------------- |
| options                | object | Options to pass to the metadata endpoint                 |
| required\_public\_keys | array  | Accounts whose public keys are required, when applicable |

## Use Cases

* **Construction Start**: Begin building a transaction from operations
* **Size Estimation**: Read the estimated transaction size
* **TTL Handling**: Obtain the relative time to live for the transaction
* **Offline Prep**: Prepare metadata options before an online fetch

## Error Handling

| Error Code | Message                   | Description                                           |
| ---------- | ------------------------- | ----------------------------------------------------- |
| 1          | Invalid request           | A required construction field is missing or malformed |
| 6          | Transaction decode failed | The supplied transaction could not be decoded         |
| 14         | Invalid signature         | A supplied signature is malformed or does not match   |
| 5          | Internal error            | The node failed to process the construction step      |
