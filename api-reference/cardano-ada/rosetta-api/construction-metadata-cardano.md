---
description: >-
  Example code for the construction/metadata JSON-RPC method. Complete guide on
  how to use construction/metadata JSON-RPC in GetBlock Web3 documentation.
---

# construction/metadata - Cardano

This endpoint returns the online metadata needed to construct a transaction, such as the current fee parameters and time-to-live, from the options produced by preprocess.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type   | Required | Description                                     |
| ------------------- | ------ | -------- | ----------------------------------------------- |
| network\_identifier | object | Yes      | The network to construct for                    |
| options             | object | Yes      | The options returned by the preprocess endpoint |
| public\_keys        | array  | No       | Public keys required for the transaction        |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/metadata' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "options": {
        "relative_ttl": 1000,
        "transaction_size": 246
    }
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/metadata',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "options": {"relative_ttl": 1000, "transaction_size": 246}})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/construction/metadata',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "options": {"relative_ttl": 1000, "transaction_size": 246}}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "metadata": {
        "ttl": 193929071,
        "protocol_parameters": {
            "coinsPerUtxoSize": "4310",
            "maxTxSize": 16384,
            "maxValSize": 5000,
            "keyDeposit": "2000000",
            "maxCollateralInputs": 3,
            "minFeeCoefficient": 44,
            "minFeeConstant": 155381,
            "minPoolCost": "170000000",
            "poolDeposit": "500000000",
            "protocol": 11
        }
    },
    "suggested_fee": [
        {
            "value": "166205",
            "currency": {
                "symbol": "ADA",
                "decimals": 6
            }
        }
    ]
}
```

## Response Parameters

| Field          | Type   | Description                                             |
| -------------- | ------ | ------------------------------------------------------- |
| metadata       | object | Metadata required to build the transaction, such as ttl |
| suggested\_fee | array  | The suggested fee for the transaction, in ada           |

## Use Cases

* **Fee Fetching**: Read the suggested fee before building payloads
* **TTL Resolution**: Resolve the absolute time to live from relative options
* **Online Step**: Fetch chain-dependent metadata for construction
* **Transaction Building**: Feed metadata into the payloads endpoint

## Error Handling

| Error Code | Message                   | Description                                           |
| ---------- | ------------------------- | ----------------------------------------------------- |
| 1          | Invalid request           | A required construction field is missing or malformed |
| 6          | Transaction decode failed | The supplied transaction could not be decoded         |
| 14         | Invalid signature         | A supplied signature is malformed or does not match   |
| 5          | Internal error            | The node failed to process the construction step      |
