---
description: >-
  Example code for the api/v2/tx REST method. Complete guide on how to use the
  api/v2/tx REST method in the GetBlock Web3 documentation.
---

# api/v2/tx- Dash

This endpoint returns a normalized transaction by its id, with inputs, outputs, addresses, and confirmation data in the indexer's unified schema.

## Parameters

| Parameter | Type    | Location | Required | Description                                                  |
| --------- | ------- | -------- | -------- | ------------------------------------------------------------ |
| txid      | string  | path     | Yes      | The transaction id                                           |
| spending  | boolean | query    | No       | Include spending transaction data for outputs when available |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/eb0351c64cde2c42bcdf10b11a1ad44bb63631bb6487de70e78b90c2aa57137c'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/eb0351c64cde2c42bcdf10b11a1ad44bb63631bb6487de70e78b90c2aa57137c'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/eb0351c64cde2c42bcdf10b11a1ad44bb63631bb6487de70e78b90c2aa57137c')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "eb0351c64cde2c42bcdf10b11a1ad44bb63631bb6487de70e78b90c2aa57137c",
    "version": 2,
    "vin": [
        {
            "txid": "3d427a54eac26a3eb54ffa7ea41172043afd64963a810e5d1119e5cb503b6184",
            "vout": 2,
            "sequence": 4294967295,
            "n": 0,
            "addresses": [
                "XqQ2uJrkHjVKvqTy6HSUNe6DMNB2KbRVMB"
            ],
            "isAddress": true,
            "value": "1000010"
        }
    ],
    "vout": [
        {
            "value": "1000010",
            "n": 0,
            "hex": "76a9144963929828681e590020340385bab2ae114bca9688ac",
            "addresses": [
                "XhNtSggaqVecdFxSFeDftR6GZk8D7Ms3JE"
            ],
            "isAddress": true
        }
    ],
    "blockHash": "00000000000000007409ab5be18f66b1f68795ff81d3b3b1d592574fae07c372",
    "blockHeight": 2540600,
    "confirmations": 74,
    "blockTime": 1789687365,
    "size": 1278,
    "value": "7000070",
    "valueIn": "7000070",
    "fees": "0"
}
```

## Response Parameters

| Field         | Type    | Description                                                  |
| ------------- | ------- | ------------------------------------------------------------ |
| txid          | string  | The transaction id                                           |
| vin           | array   | Transaction inputs with addresses and values                 |
| vout          | array   | Transaction outputs with addresses, values, and spent status |
| blockHeight   | integer | Block height, or -1 when unconfirmed                         |
| confirmations | integer | Number of confirmations                                      |
| value         | string  | Total output value in duffs                               |
| fees          | string  | Transaction fee in duffs                                  |

## Use Cases

* **Transaction Views**: Render a normalized transaction in a wallet or explorer
* **Payment Confirmation**: Read confirmations to confirm a received payment
* **Fee Inspection**: Read the computed fee for a transaction
* **Address Mapping**: Read input and output addresses without decoding raw hex
* **Spend Tracking**: Check whether outputs are spent with the spending flag

## Error Handling

| HTTP Status | Message        | Description                                               |
| ----------- | -------------- | --------------------------------------------------------- |
| 400         | Bad request    | The transaction id is not a valid 64-character hex string |
| 404         | Not found      | No transaction with the requested id is indexed           |
| 500         | Internal error | The indexer failed to read the transaction                |
