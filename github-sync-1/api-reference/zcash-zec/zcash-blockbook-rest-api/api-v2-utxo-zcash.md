---
description: >-
  Example code for the api/v2/utxo REST method. Complete guide on how to use the
  api/v2/utxo REST method in the GetBlock Web3 documentation.
---

# api/v2/utxo - Zcash

This endpoint returns the unspent transparent outputs for an address, extended public key, or descriptor. These outputs are the transparent inputs available when constructing a spending transaction.

Both confirmed and unconfirmed outputs are returned by default. Results are sorted by block height, newest first.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| addressOrXpub | string | path | Yes | Transparent address, extended public key, or descriptor. URL-encode descriptors |
| confirmed | boolean | query | No | When true, return only confirmed outputs. Default false |
| gap | integer | query | No | Derivation gap limit for xpub inputs, capped by the server |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua?confirmed=true'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua?confirmed=true'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua?confirmed=true')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "txid": "e5514dac030111c2485ae57401e52bb646c83d196b7d01a7298db98e1973228d",
        "vout": 0,
        "value": "10209258",
        "height": 3487829,
        "confirmations": 7
    }
]
```

The array is truncated above; the address returns 221 outputs.

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txid | string | Transaction id of the output |
| vout | integer | Output index within the transaction |
| value | string | Output value in zatoshis |
| height | integer | Block height at which the output was confirmed. Omitted when unconfirmed |
| confirmations | integer | Number of confirmations. 0 for unconfirmed outputs |
| address | string | Owning address. Returned only for xpub and descriptor queries |
| path | string | Derivation path of the owning address. Only for xpub and descriptor queries |
| coinbase | boolean | True for coinbase outputs, up to the coinbase maturity limit |

{% hint style="info" %}
Only transparent outputs are listed. Shielded notes are encrypted and cannot be enumerated by the indexer. The JSON-RPC [getaddressutxos](../zcash-json-rpc-api/getaddressutxos-zcash.md) method answers the same question directly from the Zebra node.
{% endhint %}

## Use Cases

* **Coin Selection**: Read spendable transparent outputs when building a transaction
* **Balance Construction**: Sum output values to compute a spendable transparent balance
* **Confirmed Filtering**: Restrict to confirmed outputs for settlement flows

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
