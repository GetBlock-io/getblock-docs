---
description: >-
  Example code for the construction/parse JSON-RPC method. Complete guide on how
  to use construction/parse JSON-RPC in GetBlock Web3 documentation.
---

# construction/parse - Cardano

This endpoint parses a transaction, signed or unsigned, back into its operations. It is used to confirm a constructed transaction matches the original intent before signing or submitting.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type    | Required | Description                                |
| ------------------- | ------- | -------- | ------------------------------------------ |
| network\_identifier | object  | Yes      | The network to parse for                   |
| signed              | boolean | Yes      | Whether the supplied transaction is signed |
| transaction         | string  | Yes      | The transaction CBOR hex to parse          |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/construction/parse' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "signed": false,
    "transaction": "a400818258200e...unsigned-cbor..."
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/construction/parse',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "signed": false, "transaction": "a400818258200e...unsigned-cbor..."})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/construction/parse',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "signed": false, "transaction": "a400818258200e...unsigned-cbor..."}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "operations": [
        {
            "operation_identifier": {
                "index": 0
            },
            "type": "output",
            "account": {
                "address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"
            },
            "amount": {
                "value": "9240398",
                "currency": {
                    "symbol": "ADA",
                    "decimals": 6
                }
            }
        }
    ],
    "account_identifier_signers": []
}
```

## Response Parameters

| Field                        | Type  | Description                                    |
| ---------------------------- | ----- | ---------------------------------------------- |
| operations                   | array | The operations decoded from the transaction    |
| account\_identifier\_signers | array | Accounts that signed, for a signed transaction |

## Use Cases

* **Intent Verification**: Confirm a built transaction matches the intent
* **Signer Checks**: Read which accounts signed a transaction
* **Debugging**: Inspect the operations of a constructed transaction
* **Safety**: Validate a transaction before submitting it

## Error Handling

| Error Code | Message                   | Description                                           |
| ---------- | ------------------------- | ----------------------------------------------------- |
| 1          | Invalid request           | A required construction field is missing or malformed |
| 6          | Transaction decode failed | The supplied transaction could not be decoded         |
| 14         | Invalid signature         | A supplied signature is malformed or does not match   |
| 5          | Internal error            | The node failed to process the construction step      |
