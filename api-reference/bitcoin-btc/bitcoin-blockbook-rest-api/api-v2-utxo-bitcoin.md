---
description: >-
  Example code for the api/v2/utxo REST method. Complete guide on how to use the
  api/v2/utxo REST method in the GetBlock Web3 documentation.
---

# api/v2/utxo - Bitcoin

This endpoint returns the unspent transaction outputs for an address, extended public key, or descriptor. These outputs are the inputs available when constructing a spending transaction.

Both confirmed and unconfirmed outputs are returned by default. Unconfirmed outputs omit `height` and report `confirmations` as 0. Results are sorted by block height, newest first.

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?confirmed=true'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?confirmed=true'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?confirmed=true')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
        "vout": 0,
        "value": "50000000",
        "height": 830000,
        "confirmations": 152,
        "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
        "path": "m/84'/0'/0'/0/0"
    }
]
```

## Response Parameters

| Field         | Type    | Description                                                            |
| ------------- | ------- | ----------------------------------------------------------------------- |
| txid          | string  | Transaction id of the output                                            |
| vout          | integer | Output index within the transaction                                     |
| value         | string  | Output value in satoshis                                                |
| height        | integer | Block height at which the output was confirmed. Omitted when unconfirmed |
| confirmations | integer | Number of confirmations. 0 for unconfirmed outputs                      |
| address       | string  | Address that owns the output, for xpub and descriptor queries           |
| path          | string  | Derivation path of the owning address, for xpub and descriptor queries  |
| lockTime      | integer | Lock time, present on unconfirmed outputs that set one                  |
| coinbase      | boolean | True for coinbase outputs, up to the 100-block coinbase maturity limit  |

## Use Cases

* **Coin Selection**: Read spendable outputs when building a transaction
* **Balance Construction**: Sum output values to compute a spendable balance
* **Wallet Funding**: Find the UTXOs behind a wallet before spending
* **Confirmed Filtering**: Restrict to confirmed outputs for settlement flows
* **Payment Processing**: Track which outputs are available to sweep or consolidate

{% hint style="info" %}
This endpoint replaces the Bitcoin Core `listunspent` wallet RPC, which is disabled on shared nodes because those nodes carry no user wallets. It is also the equivalent of the upstream Blockbook `bb_getUTXOs` method, which GetBlock does not expose over JSON-RPC.
{% endhint %}

## Error Handling

| HTTP Status | Message        | Description                                      |
| ----------- | -------------- | ------------------------------------------------ |
| 400         | Bad request    | The address, XPUB, or descriptor is malformed    |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN                  |
| 404         | Not found      | No indexed data exists for the requested account |
| 500         | Internal error | The indexer failed to read account data          |
