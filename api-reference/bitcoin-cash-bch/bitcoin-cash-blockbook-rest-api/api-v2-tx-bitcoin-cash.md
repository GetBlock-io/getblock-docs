---
description: >-
  Example code for the api/v2/tx REST method. Complete guide on how to use the
  api/v2/tx REST method in the GetBlock Web3 documentation.
---

# api/v2/tx- Bitcoin Cash

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
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tx/10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tx/10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tx/10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
    "version": 1,
    "vin": [
        {
            "txid": "780791bb2d5a8ccda4b5a707967a8e15b412814852c58c77299e85579bb65587",
            "vout": 1,
            "sequence": 4294967295,
            "n": 0,
            "addresses": [
                "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a"
            ],
            "isAddress": true,
            "value": "12345970"
        }
    ],
    "vout": [
        {
            "value": "9407625",
            "n": 0,
            "spent": false,
            "addresses": [
                "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a"
            ],
            "isAddress": true,
            "type": "pubkeyhash"
        }
    ],
    "blockHash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
    "blockHeight": 684634,
    "confirmations": 1197,
    "blockTime": 1617180599,
    "size": 226,
    "value": "9407625",
    "valueIn": "12345970",
    "fees": "2938345"
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
| value         | string  | Total output value in satoshis                               |
| fees          | string  | Transaction fee in satoshis                                  |

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
