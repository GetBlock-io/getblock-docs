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
        "txid": "1d51c696eab35d7dad7631516cc0845aaab317fe67cb182782752547541d30ea",
        "vout": 0,
        "value": "32323",
        "height": 967397,
        "confirmations": 91
    },
    {
        "txid": "a6494142e2e565b5e672d41a37a3eafec2fe5594f22efbb07f421a6cedf473c5",
        "vout": 1,
        "value": "100000",
        "height": 966724,
        "confirmations": 764
    }
]
```

For an address query the outputs carry no `address` or `path`, because the owning address is the one in the request. Querying an xpub or descriptor adds both, identifying which derived address owns each output:

```json
[
    {
        "txid": "88aa69107bccdddb23df8b2635ff759cdc5e6873b91bef91325f70060973299a",
        "vout": 0,
        "value": "2446",
        "height": 906576,
        "confirmations": 60912,
        "address": "1AWhq6hMWzwxEG1wGeR7Y9aTyoxEjw7Rjj",
        "path": "m/44'/0'/0'/0/15"
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
