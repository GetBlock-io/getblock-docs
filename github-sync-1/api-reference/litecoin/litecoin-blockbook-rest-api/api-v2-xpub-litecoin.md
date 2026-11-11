---
description: >-
  Example code for the api/v2/xpub REST method. Complete guide on how to use the
  api/v2/xpub REST method in the GetBlock Web3 documentation.
---

# api/v2/xpub - Litecoin

This endpoint returns wallet-level balance and transaction data for an extended public key or output descriptor. Blockbook derives the addresses from the key and returns the combined result, so each address does not have to be tracked separately.

Litecoin derives at BIP44 coin type 2, so its account paths read `m/44'/2'/0'/...`. The BIP scheme is inferred from the key prefix.

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz?details=tokens&tokens=used'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz?details=tokens&tokens=used'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz?details=tokens&tokens=used')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "address": "xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz",
    "balance": "2680044",
    "totalReceived": "2680044",
    "totalSent": "0",
    "unconfirmedBalance": "0",
    "unconfirmedTxs": 0,
    "txs": 1,
    "addrTxCount": 1,
    "usedTokens": 1,
    "tokens": [
        {
            "type": "XPUBAddress",
            "standard": "XPUBAddress",
            "name": "LLwLECw5AyVcFsB8bbXVhvtfAxgiYr1v5Q",
            "path": "m/44'/2'/0'/0/1",
            "transfers": 1,
            "decimals": 8
        }
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| address | string | The queried extended public key or descriptor |
| balance | string | Combined confirmed balance of the wallet in litoshis |
| totalReceived | string | Combined total received in litoshis |
| totalSent | string | Combined total sent in litoshis |
| txs | integer | Number of transactions across the derived addresses |
| usedTokens | integer | Total number of used derived addresses |
| tokens | array | Derived address rows, each with its own path and transfer count |
| tokens[].name | string | The derived address |
| tokens[].path | string | BIP32 derivation path of that address |

{% hint style="info" %}
`usedTokens` always reports the total number of used addresses for the key, regardless of the `tokens` filter. Blockbook expects the key at level 3 of the derivation path, for example `m/purpose'/coin_type'/account'`, and derives the remaining change and index levels itself.
{% endhint %}

## Use Cases

* **Wallet Balances**: Read a whole wallet in one call instead of per address
* **History Aggregation**: Collect transactions across every derived address
* **Address Discovery**: List which derived addresses have been used
* **Accounting**: Reconcile wallet-level totals

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The extended public key or descriptor is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 500 | Internal error | The indexer failed to derive or read the account |
