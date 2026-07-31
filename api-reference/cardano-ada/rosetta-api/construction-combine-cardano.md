---
description: >-
  Example code for the construction/combine JSON_RPC method. Complete guide on
  how to use construction/combineJSON_RPC in GetBlock Web3 documentation.
---

# construction/combine - Cardano

This endpoint combines an unsigned transaction with its signatures to produce a signed transaction. It is an offline step in the construction flow.

## Parameters

The request body is a JSON object with the following fields.

| Field                 | Type   | Required | Description                              |
| --------------------- | ------ | -------- | ---------------------------------------- |
| network\_identifier   | object | Yes      | The network to construct for             |
| unsigned\_transaction | string | Yes      | The unsigned transaction CBOR hex        |
| signatures            | array  | Yes      | The signatures produced for the payloads |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/construction/combine' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "unsigned_transaction": "a400818258200e...unsigned-cbor...",
    "signatures": [
        {
            "signing_payload": {
                "account_identifier": {
                    "address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"
                },
                "hex_bytes": "3e6f2d8c...signing-payload"
            },
            "public_key": {
                "hex_bytes": "b3c2a1d0e9f8...ed25519-public-key-hex",
                "curve_type": "edwards25519"
            },
            "signature_type": "ed25519",
            "hex_bytes": "a1b2c3...signature"
        }
    ]
}'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/construction/combine',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "unsigned_transaction": "a400818258200e...unsigned-cbor...", "signatures": [{"signing_payload": {"account_identifier": {"address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"}, "hex_bytes": "3e6f2d8c...signing-payload"}, "public_key": {"hex_bytes": "b3c2a1d0e9f8...ed25519-public-key-hex", "curve_type": "edwards25519"}, "signature_type": "ed25519", "hex_bytes": "a1b2c3...signature"}]})
    }
);
console.log(await response.json());
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/construction/combine',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "unsigned_transaction": "a400818258200e...unsigned-cbor...", "signatures": [{"signing_payload": {"account_identifier": {"address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"}, "hex_bytes": "3e6f2d8c...signing-payload"}, "public_key": {"hex_bytes": "b3c2a1d0e9f8...ed25519-public-key-hex", "curve_type": "edwards25519"}, "signature_type": "ed25519", "hex_bytes": "a1b2c3...signature"}]}
)

print(response.json())
```
{% endtab %}
{% endtabs %}

## Response

```json
{
    "signed_transaction": "84a400818258200e...signed-cbor..."
}
```

## Response Parameters

| Field               | Type   | Description                        |
| ------------------- | ------ | ---------------------------------- |
| signed\_transaction | string | The signed transaction in CBOR hex |

## Use Cases

* **Signature Assembly**: Attach signatures to an unsigned transaction
* **Offline Signing**: Combine signatures produced on an air-gapped device
* **Multi-Signature**: Assemble signatures from several signers
* **Construction Flow**: Advance to the hash and submit steps

## Error Handling

| Error Code | Message                   | Description                                           |
| ---------- | ------------------------- | ----------------------------------------------------- |
| 1          | Invalid request           | A required construction field is missing or malformed |
| 6          | Transaction decode failed | The supplied transaction could not be decoded         |
| 14         | Invalid signature         | A supplied signature is malformed or does not match   |
| 5          | Internal error            | The node failed to process the construction step      |
