---
description: >-
  Example code for the api/v2/utxo REST method. Complete guide on how to use the
  api/v2/utxo REST method in the GetBlock Web3 documentation.
---

# api/v2/utxo - Litecoin

This endpoint returns the unspent transaction outputs for an address, extended public key, or descriptor. These outputs are the inputs available when constructing a spending transaction.

Both confirmed and unconfirmed outputs are returned by default. Results are sorted by block height, newest first.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| addressOrXpub | string | path | Yes | Address, extended public key, or descriptor. URL-encode descriptors |
| confirmed | boolean | query | No | When true, return only confirmed outputs. Default false |
| gap | integer | query | No | Derivation gap limit for xpub inputs, capped by the server |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5?confirmed=true'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5?confirmed=true'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5?confirmed=true')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "txid": "51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969",
        "vout": 0,
        "value": "8324080358",
        "height": 3179800,
        "confirmations": 17
    },
    {
        "txid": "4c3d9548bb03a42551c10f2472e99cd86776ed00fd3bfb530c4466b93e7ddac7",
        "vout": 0,
        "value": "8080253532",
        "height": 3179209,
        "confirmations": 608
    }
]
```

An address query returns no `address` or `path`, because the owning address is the one in the request. Querying an xpub or descriptor adds both, identifying which derived address owns each output:

```json
[
    {
        "txid": "da184ddc2f8f641765bc8b6073be10f4fed8e7bdc4c40cbddc5e86939ce6d3ab",
        "vout": 0,
        "value": "2680044",
        "height": 2923739,
        "confirmations": 256078,
        "address": "LLwLECw5AyVcFsB8bbXVhvtfAxgiYr1v5Q",
        "path": "m/44'/2'/0'/0/1"
    }
]
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txid | string | Transaction id of the output |
| vout | integer | Output index within the transaction |
| value | string | Output value in litoshis |
| height | integer | Block height at which the output was confirmed. Omitted when unconfirmed |
| confirmations | integer | Number of confirmations. 0 for unconfirmed outputs |
| address | string | Owning address. Returned only for xpub and descriptor queries |
| path | string | Derivation path of the owning address. Only for xpub and descriptor queries |
| coinbase | boolean | True for coinbase outputs, up to the coinbase maturity limit |

{% hint style="info" %}
This endpoint replaces the Litecoin Core `listunspent` wallet RPC, which is disabled on shared nodes because those nodes carry no user wallets.
{% endhint %}

## Use Cases

* **Coin Selection**: Read spendable outputs when building a transaction
* **Balance Construction**: Sum output values to compute a spendable balance
* **Wallet Funding**: Find the UTXOs behind a wallet before spending
* **Confirmed Filtering**: Restrict to confirmed outputs for settlement flows

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The address, XPUB, or descriptor is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the requested account |
| 500 | Internal error | The indexer failed to read account data |
