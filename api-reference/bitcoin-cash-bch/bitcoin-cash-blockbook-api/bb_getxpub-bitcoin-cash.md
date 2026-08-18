---
description: >-
  Example code for the bb_getXpub JSON-RPC method. Complete guide on how to use
  the bb_getXpub JSON-RPC method in the GetBlock Web3 documentation.
---

# bb\_getXpub - Bitcoin Cash

This method returns wallet-level balance and transaction data for an extended public key or output descriptor. Blockbook derives the wallet's addresses and returns the combined result, so each address does not have to be queried individually.

## Parameters

| Parameter | Type    | Required | Description                                                                 |
| --------- | ------- | -------- | --------------------------------------------------------------------------- |
| xpub      | string  | Yes      | The extended public key or supported output descriptor                      |
| page      | integer | No       | 1-based page index for transaction history. Default 1                       |
| pageSize  | integer | No       | Number of history items per page. Default and maximum is 1000               |
| from      | integer | No       | First block height to include when filtering history                        |
| to        | integer | No       | Last block height to include when filtering history                         |
| details   | string  | No       | Level of detail: basic, tokens, tokenBalances, txids, or txs. Default txids |
| tokens    | string  | No       | Which derived addresses to include: nonzero, used, or derived               |
| gap       | integer | No       | Derivation gap limit, capped by the server                                  |
| secondary | string  | No       | Secondary fiat currency code to include converted values                    |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz?details=tokens&tokens=used'
```
{% endcode %}
{% endtab %}

{% tab title="cURL (JSON-RPC)" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "bb_getXpub",
    "params": [
        "xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz",
        {
            "page": 1,
            "size": 1000,
            "details": "tokens",
            "tokens": "used"
        }
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz?details=tokens&tokens=used'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
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
    "page": 1,
    "totalPages": 1,
    "itemsOnPage": 1000,
    "address": "xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz",
    "balance": "18452100",
    "totalReceived": "58452100",
    "totalSent": "40000000",
    "unconfirmedBalance": "0",
    "unconfirmedTxs": 0,
    "txs": 12,
    "usedTokens": 3,
    "tokens": [
        {
            "type": "XPUBAddress",
            "name": "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a",
            "path": "m/44'/145'/0'/0/0",
            "transfers": 7,
            "balance": "9407625",
            "totalReceived": "45231890",
            "totalSent": "35824265"
        }
    ]
}
```

## Response Parameters

| Field         | Type    | Description                                                       |
| ------------- | ------- | ----------------------------------------------------------------- |
| balance       | string  | Combined confirmed balance of the wallet in satoshis              |
| totalReceived | string  | Combined total received in satoshis                               |
| totalSent     | string  | Combined total sent in satoshis                                   |
| txs           | integer | Number of confirmed transactions across the wallet                |
| usedTokens    | integer | Number of derived addresses that have been used                   |
| tokens        | array   | Derived address rows, each with path, balance, and transfer count |

## Use Cases

* **Wallet Balances**: Return a full wallet balance from one xpub call
* **Address Derivation**: List derived addresses without deriving them locally
* **Portfolio Trackers**: Aggregate wallet activity across legacy and derived addresses
* **Gap Control**: Tune the derivation gap limit for wallets with sparse usage
* **Receive Flows**: Find the next unused address from the derived rows

## Error Handling

| Error Code   | Message        | Description                                      |
| ------------ | -------------- | ------------------------------------------------ |
| 400 / -32602 | Bad request    | The address, XPUB, or descriptor is malformed    |
| 404 / -32603 | Not found      | No indexed data exists for the requested account |
| 500 / -32603 | Internal error | The indexer failed to read account data          |
