---
description: >-
  Example code for the api/v2/xpub REST method. Complete guide on how to use the
  api/v2/xpub REST method in the GetBlock Web3 documentation.
---

# api/v2/xpub - Dogecoin

This endpoint returns wallet-level balance and transaction data for an extended public key or output descriptor. Blockbook derives the addresses from the key and returns the combined result, so each address does not have to be tracked separately.

Dogecoin derives at BIP44 coin type 3, so its account paths read `m/44'/3'/0'/...`.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| xpub | string | path | Yes | Extended public key or supported output descriptor. URL-encode descriptors |
| details | string | query | No | Detail level: basic, tokens, tokenBalances, txids, txslight, or txs |
| tokens | string | query | No | Which derived addresses to include: nonzero, used, or derived. Default nonzero |
| page | integer | query | No | 1-based page index for transaction history |
| pageSize | integer | query | No | History items per page. Default and maximum is 1000 |
| gap | integer | query | No | Derivation gap limit, capped by the server |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/<XPUB>?details=tokens&tokens=used'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/<XPUB>?details=tokens&tokens=used'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/<XPUB>?details=tokens&tokens=used')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "address": "<XPUB>",
    "balance": "1105234012375840",
    "totalReceived": "1631025972011211333",
    "totalSent": "1629920737998835493",
    "unconfirmedBalance": "0",
    "unconfirmedTxs": 0,
    "txs": 26270,
    "usedTokens": 2,
    "tokens": [
        {
            "type": "XPUBAddress",
            "standard": "XPUBAddress",
            "name": "DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF",
            "path": "m/44'/3'/0'/0/0",
            "transfers": 26270,
            "decimals": 8
        }
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| address | string | The queried extended public key or descriptor |
| balance | string | Combined confirmed balance of the wallet in koinu |
| totalReceived | string | Combined total received in koinu |
| totalSent | string | Combined total sent in koinu |
| txs | integer | Number of transactions across the derived addresses |
| usedTokens | integer | Total number of used derived addresses |
| tokens | array | Derived address rows, each with its own path and transfer count |
| tokens[].name | string | The derived address |
| tokens[].path | string | BIP32 derivation path of that address |

{% hint style="warning" %}
The response above is illustrative: it shows the shape returned for a wallet with history, using real figures from the address it derives. It was **not** captured from a live Dogecoin xpub query.

The endpoint accepts any well-formed extended public key, but returns a zeroed account for one that has never been used on Dogecoin. Substitute an xpub from a Dogecoin wallet to see real values. `usedTokens` always reports the total number of used addresses regardless of the `tokens` filter.
{% endhint %}

## Use Cases

* **Wallet Balances**: Read a whole wallet in one call instead of per address
* **History Aggregation**: Collect transactions across every derived address
* **Address Discovery**: List which derived addresses have been used
* **Accounting**: Reconcile wallet-level totals

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
