---
description: >-
  Example code for the construction/submit JSON-RPC method. Complete guide on
  how to use construction/submit JSON-RPC in GetBlock Web3 documentation.
---

# construction/submit - Cardano

This endpoint broadcasts a signed transaction to the network and returns its transaction hash on acceptance. It is the final, online step of the construction flow.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type   | Required | Description                     |
| ------------------- | ------ | -------- | ------------------------------- |
| network\_identifier | object | Yes      | The network to submit to        |
| signed\_transaction | string | Yes      | The signed transaction CBOR hex |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/submit' \
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/submit',
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/submit',
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

| Field                   | Type   | Description                          |
| ----------------------- | ------ | ------------------------------------ |
| transaction\_identifier | object | The hash of the accepted transaction |

## Use Cases

* **Payment Broadcast**: Submit a signed transaction to the network
* **dApp Backends**: Broadcast transactions built through the construction flow
* **Exchange Withdrawals**: Submit signed withdrawal transactions
* **Retry Flows**: Resubmit a signed transaction from stored CBOR

## Error Handling

| Error Code | Message                   | Description                                         |
| ---------- | ------------------------- | --------------------------------------------------- |
| 1          | Invalid request           | The signed transaction is missing or malformed      |
| 6          | Transaction decode failed | The signed transaction could not be decoded         |
| 9          | Transaction rejected      | The node rejected the transaction during submission |
| 5          | Internal error            | The node failed to broadcast the transaction        |
