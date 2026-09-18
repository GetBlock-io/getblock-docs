---
description: >-
  Example code for the api/v2/utxo REST method. Complete guide on how to use the
  api/v2/utxo REST method in the GetBlock Web3 documentation.
---

# api/v2/utxo - Dash

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/XjszN1jZJthEoaQDhGthRkaHL9AqaG3Vzw?confirmed=true'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/XjszN1jZJthEoaQDhGthRkaHL9AqaG3Vzw?confirmed=true'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/XjszN1jZJthEoaQDhGthRkaHL9AqaG3Vzw?confirmed=true')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "txid": "092fee8b2d026eb7959faa3e3c20dcc37c84b24378abf8904fc775b67b2a197a",
        "vout": 0,
        "value": "41098577",
        "height": 2540673,
        "confirmations": 1,
        "coinbase": true
    },
    {
        "txid": "28fd12e061c1e086e9861cfd396e6bc1afcdfc28c245cb487e4fe421cefbd926",
        "vout": 0,
        "value": "41098317",
        "height": 2540671,
        "confirmations": 3,
        "coinbase": true
    }
]
```

An address query returns no `address` or `path`, because the owning address is the one in the request. Querying an xpub or descriptor adds both to each output, identifying which derived address owns it.

## Response Parameters

| Field         | Type    | Description                                                              |
| ------------- | ------- | -------------------------------------------------------------------------- |
| txid          | string  | Transaction id of the output                                             |
| vout          | integer | Output index within the transaction                                      |
| value         | string  | Output value in duffs                                                    |
| height        | integer | Block height at which the output was confirmed. Omitted when unconfirmed |
| confirmations | integer | Number of confirmations. 0 for unconfirmed outputs                       |
| address       | string  | Owning address. Returned only for xpub and descriptor queries            |
| path          | string  | Derivation path of the owning address. Only for xpub and descriptor queries |
| coinbase      | boolean | True for coinbase outputs, up to the 100-block coinbase maturity limit    |

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
