---
description: >-
  Example code for the api/v2/utxo REST method. Complete guide on how to use the
  api/v2/utxo REST method in the GetBlock Web3 documentation.
---

# api/v2/utxo - Dogecoin

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF?confirmed=true'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF?confirmed=true'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF?confirmed=true')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "txid": "4c5519699240ed1f23e1ec46ea4bc85f019982f6ef48afdb7698054230a4b76e",
        "vout": 0,
        "value": "1000000",
        "height": 6378960,
        "confirmations": 2
    },
    {
        "txid": "8d99a37f27d702b35bcc74dfea0d1a006a67c20571e1de660a30defc16316c8f",
        "vout": 1,
        "value": "248941280401251",
        "height": 6378956,
        "confirmations": 6
    }
]
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txid | string | Transaction id of the output |
| vout | integer | Output index within the transaction |
| value | string | Output value in koinu |
| height | integer | Block height at which the output was confirmed. Omitted when unconfirmed |
| confirmations | integer | Number of confirmations. 0 for unconfirmed outputs |
| address | string | Owning address. Returned only for xpub and descriptor queries |
| path | string | Derivation path of the owning address. Only for xpub and descriptor queries |
| coinbase | boolean | True for coinbase outputs, up to the coinbase maturity limit |

{% hint style="info" %}
An address query returns no `address` or `path`; those fields appear only for xpub and descriptor queries, where they identify which derived address owns each output.

This endpoint replaces the Dogecoin Core `listunspent` wallet RPC, which is disabled on shared nodes because those nodes carry no user wallets.
{% endhint %}

## Use Cases

* **Coin Selection**: Read spendable outputs when building a transaction
* **Balance Construction**: Sum output values to compute a spendable balance
* **Wallet Funding**: Find the UTXOs behind a wallet before spending
* **Confirmed Filtering**: Restrict to confirmed outputs for settlement flows

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
