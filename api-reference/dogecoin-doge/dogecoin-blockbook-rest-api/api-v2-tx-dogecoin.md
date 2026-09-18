---
description: >-
  Example code for the api/v2/tx REST method. Complete guide on how to use the
  api/v2/tx REST method in the GetBlock Web3 documentation.
---

# api/v2/tx - Dogecoin

This endpoint returns a normalized transaction by its id, with inputs, outputs, addresses, values, and confirmation data in the indexer's unified schema.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| txid | string | path | Yes | The transaction id |
| spending | boolean | query | No | Include spending transaction data for outputs when available |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txid": "d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed",
    "version": 1,
    "vin": [
        {
            "txid": "9af546f78a4cae9031954de80203cff6100c9c22744a4827738713595a1c588a",
            "sequence": 4294967295,
            "n": 0,
            "addresses": [
                "DL4iyB4jtVNtwPsti24ron4prY2UZoUjG3"
            ],
            "isAddress": true,
            "value": "500000000000000"
        }
    ],
    "vout": [
        {
            "value": "180647818052655",
            "n": 0,
            "spent": true,
            "hex": "76a914986ae75df449ca5e04503c0baa69bbeca93fcf6188ac",
            "addresses": [
                "DK31KDnpWXT5DyhZgrYY86VdQ92pp7hXmP"
            ],
            "isAddress": true
        }
    ],
    "blockHash": "afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf",
    "blockHeight": 6378930,
    "confirmations": 32,
    "blockTime": 1789703885,
    "size": 225,
    "value": "499999999659500",
    "valueIn": "500000000000000",
    "fees": "340500"
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txid | string | The transaction id |
| vin | array | Inputs with addresses and values |
| vout | array | Outputs with values, scripts, and destination addresses |
| vout[].spent | boolean | True when the output has already been spent |
| blockHash | string | Hash of the containing block. Absent while unconfirmed |
| blockHeight | integer | Block height, or -1 while unconfirmed |
| confirmations | integer | Number of confirmations. 0 while unconfirmed |
| size | integer | Serialized size in bytes |
| value | string | Total output value in koinu |
| valueIn | string | Total input value in koinu |
| fees | string | Fee paid in koinu |

{% hint style="info" %}
Dogecoin has no SegWit, so there is no `vsize` field; `size` is the only size reported and fee rates are per byte. Empty fields are omitted rather than returned as null, so `lockTime` and `rbf` are absent unless set.
{% endhint %}

## Use Cases

* **Transaction Views**: Render a normalized transaction in a wallet or explorer
* **Payment Confirmation**: Read confirmations to confirm a received payment
* **Fee Inspection**: Read the computed fee for a transaction
* **Spend Tracking**: Check whether outputs are already spent

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
