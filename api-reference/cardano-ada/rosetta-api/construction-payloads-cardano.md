---
description: >-
  Example code for the construction/payloads JSON-RPC method. Complete guide on
  how to use construction/payloads JSON-RPC in GetBlock Web3 documentation.
---

# construction/payloads - Cardano

This endpoint builds an unsigned transaction and the payloads that must be signed, from the operations and metadata. It is an offline step that returns the material to sign.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type   | Required | Description                                |
| ------------------- | ------ | -------- | ------------------------------------------ |
| network\_identifier | object | Yes      | The network to construct for               |
| operations          | array  | Yes      | The operations describing the transaction  |
| metadata            | object | Yes      | Metadata returned by the metadata endpoint |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/payloads' \
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
    "metadata": {
        "ttl": "10454789"
    }
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/payloads',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "operations": [{"operation_identifier": {"index": 0}, "type": "output", "account": {"address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"}, "amount": {"value": "9240398", "currency": {"symbol": "ADA", "decimals": 6}}}], "metadata": {"ttl": "10454789"}})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/payloads',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "operations": [{"operation_identifier": {"index": 0}, "type": "output", "account": {"address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"}, "amount": {"value": "9240398", "currency": {"symbol": "ADA", "decimals": 6}}}], "metadata": {"ttl": "10454789"}}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "unsigned_transaction": "a400818258200e...unsigned-cbor...",
    "payloads": [
        {
            "account_identifier": {
                "address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"
            },
            "hex_bytes": "3e6f2d8c...signing-payload",
            "signature_type": "ed25519"
        }
    ]
}
```

## Response Parameters

| Field                 | Type   | Description                                           |
| --------------------- | ------ | ----------------------------------------------------- |
| unsigned\_transaction | string | The unsigned transaction in CBOR hex                  |
| payloads              | array  | The payloads to sign, each with the account and bytes |

## Use Cases

* **Transaction Building**: Produce an unsigned transaction from operations
* **Offline Signing**: Return payloads to sign on an air-gapped device
* **Multi-Signature**: Enumerate the payloads each signer must sign
* **Construction Flow**: Advance the flow to the parse and combine steps

## Error Handling

| Error Code | Message                   | Description                                           |
| ---------- | ------------------------- | ----------------------------------------------------- |
| 1          | Invalid request           | A required construction field is missing or malformed |
| 6          | Transaction decode failed | The supplied transaction could not be decoded         |
| 14         | Invalid signature         | A supplied signature is malformed or does not match   |
| 5          | Internal error            | The node failed to process the construction step      |
