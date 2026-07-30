---
description: >-
  Example code for the api/v2/utxo REST method. Complete guide on how to use the
  api/v2/utxo REST method in the GetBlock Web3 documentation.
---

# api/v2/utxo  - Bitcoin Cash

This endpoint returns the unspent transaction outputs for an address, extended public key, or descriptor. These outputs are the inputs available when constructing a spending transaction.

## Parameters

| Parameter     | Type    | Location | Required | Description                                                         |
| ------------- | ------- | -------- | -------- | ------------------------------------------------------------------- |
| addressOrXpub | string  | path     | Yes      | Address, extended public key, or descriptor. URL-encode descriptors |
| confirmed     | boolean | query    | No       | When true, return only confirmed outputs. Default false             |
| gap           | integer | query    | No       | Derivation gap limit for xpub inputs, capped by the server          |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?confirmed=true'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?confirmed=true'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?confirmed=true')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "txid": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
        "vout": 1,
        "value": "9407625",
        "height": 684634,
        "confirmations": 1197,
        "address": "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a",
        "path": "m/44'/145'/0'/0/0"
    }
]
```

## Response Parameters

| Field         | Type    | Description                                    |
| ------------- | ------- | ---------------------------------------------- |
| txid          | string  | Transaction id of the output                   |
| vout          | integer | Output index within the transaction            |
| value         | string  | Output value in satoshis                       |
| height        | integer | Block height at which the output was confirmed |
| confirmations | integer | Number of confirmations                        |
| address       | string  | Address that owns the output, for xpub queries |

## Use Cases

* **Coin Selection**: Read spendable outputs when building a transaction
* **Balance Construction**: Sum output values to compute a spendable balance
* **Wallet Funding**: Find the UTXOs behind a wallet before spending
* **Confirmed Filtering**: Restrict to confirmed outputs for settlement flows

## Error Handling

| HTTP Status | Message        | Description                                      |
| ----------- | -------------- | ------------------------------------------------ |
| 400         | Bad request    | The address, XPUB, or descriptor is malformed    |
| 404         | Not found      | No indexed data exists for the requested account |
| 500         | Internal error | The indexer failed to read account data          |
