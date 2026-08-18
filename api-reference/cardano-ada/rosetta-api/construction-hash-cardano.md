---
description: >-
  Example code for the construction/hash JSON-RPC method. Complete guide on how
  to use construction/hash JSON-RPC in GetBlock Web3 documentation.
---

# construction/hash - Cardano

This endpoint returns the transaction hash of a signed transaction. The hash is the identifier the transaction will have once submitted.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type   | Required | Description                     |
| ------------------- | ------ | -------- | ------------------------------- |
| network\_identifier | object | Yes      | The network to construct for    |
| signed\_transaction | string | Yes      | The signed transaction CBOR hex |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/hash' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "signed_transaction": "84a400818258200e...signed-cbor..."
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/hash',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "signed_transaction": "84a400818258200e...signed-cbor..."})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/hash',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "signed_transaction": "84a400818258200e...signed-cbor..."}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "transaction_identifier": {
        "hash": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"
    }
}
```

## Response Parameters

| Field                   | Type   | Description                                       |
| ----------------------- | ------ | ------------------------------------------------- |
| transaction\_identifier | object | The hash the transaction will have once submitted |

## Use Cases

* **Pre-Submit Hashing**: Compute the transaction hash before broadcasting
* **Tracking Setup**: Record the hash to track the transaction later
* **Idempotency**: Detect a duplicate submission by hash
* **Receipts**: Return a transaction hash to a user before submission

## Error Handling

| Error Code | Message                   | Description                                           |
| ---------- | ------------------------- | ----------------------------------------------------- |
| 1          | Invalid request           | A required construction field is missing or malformed |
| 6          | Transaction decode failed | The supplied transaction could not be decoded         |
| 14         | Invalid signature         | A supplied signature is malformed or does not match   |
| 5          | Internal error            | The node failed to process the construction step      |
