---
description: >-
  Example code for the api/v2/xpub REST method. Complete guide on how to use the
  api/v2/xpub REST method in the GetBlock Web3 documentation.
---

# api/v2/xpub - Zcash

This endpoint returns wallet-level balance and transaction data for an extended public key or output descriptor, derived over transparent addresses. Zcash derives transparent accounts at BIP44 coin type 133, so paths read `m/44'/133'/0'/...`.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| xpub | string | path | Yes | Extended public key or supported output descriptor. URL-encode descriptors |
| details | string | query | No | Detail level: basic, tokens, tokenBalances, txids, txslight, or txs |
| tokens | string | query | No | Which derived addresses to include: nonzero, used, or derived. Default nonzero |
| gap | integer | query | No | Derivation gap limit, capped by the server |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/<XPUB>?details=basic'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/<XPUB>?details=basic'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/<XPUB>?details=basic')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "address": "<XPUB>",
    "balance": "0",
    "totalReceived": "0",
    "totalSent": "0",
    "unconfirmedBalance": "0",
    "unconfirmedTxs": 0,
    "txs": 0
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| address | string | The queried extended public key or descriptor |
| balance | string | Combined transparent balance of the wallet in zatoshis |
| totalReceived | string | Combined total received in zatoshis |
| totalSent | string | Combined total sent in zatoshis |
| txs | integer | Number of transactions across the derived addresses |
| tokens | array | Derived address rows, present when details is tokens or higher |

{% hint style="warning" %}
This covers **transparent** derivation only. Shielded Sapling and Orchard accounts are derived from spending and viewing keys under ZIP-32, not from a BIP32 extended public key, and are not indexed. The response shown is the zeroed account returned for a key with no Zcash history; substitute an xpub from a Zcash wallet's transparent account to see its balances.
{% endhint %}

## Use Cases

* **Wallet Balances**: Read a whole transparent wallet in one call
* **Address Discovery**: List which derived addresses have been used

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
