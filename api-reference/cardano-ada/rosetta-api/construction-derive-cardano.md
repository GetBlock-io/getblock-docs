---
description: >-
  Example code for the construction/derive JSON-RPC method. Complete guide on
  how to use construction/derive JSON-RPC in GetBlock Web3 documentation.
---

# construction/derive - Cardano

This endpoint derives the account address associated with a public key. It is an offline step in the transaction construction flow.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type   | Required | Description                                        |
| ------------------- | ------ | -------- | -------------------------------------------------- |
| network\_identifier | object | Yes      | The network to derive for                          |
| public\_key         | object | Yes      | The public key, with hex\_bytes and curve\_type    |
| metadata            | object | No       | Optional derivation metadata, such as address type |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/construction/derive' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "public_key": {
        "hex_bytes": "b3c2a1d0e9f8...ed25519-public-key-hex",
        "curve_type": "edwards25519"
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
    'https://go.getblock.io/<ACCESS-TOKEN>/construction/derive',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "public_key": {"hex_bytes": "b3c2a1d0e9f8...ed25519-public-key-hex", "curve_type": "edwards25519"}, "metadata": {}})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/construction/derive',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "public_key": {"hex_bytes": "b3c2a1d0e9f8...ed25519-public-key-hex", "curve_type": "edwards25519"}, "metadata": {}}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "account_identifier": {
        "address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"
    }
}
```

## Response Parameters

| Field               | Type   | Description                                |
| ------------------- | ------ | ------------------------------------------ |
| account\_identifier | object | The derived account, with an address field |

## Use Cases

* **Address Derivation**: Derive an address from a public key offline
* **Wallet Setup**: Generate receive addresses for a key
* **Verification**: Confirm an address matches a known key
* **Offline Flows**: Derive addresses on an air-gapped device

## Error Handling

| Error Code | Message                   | Description                                           |
| ---------- | ------------------------- | ----------------------------------------------------- |
| 1          | Invalid request           | A required construction field is missing or malformed |
| 6          | Transaction decode failed | The supplied transaction could not be decoded         |
| 14         | Invalid signature         | A supplied signature is malformed or does not match   |
| 5          | Internal error            | The node failed to process the construction step      |
