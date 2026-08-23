# api v2 xpub bitcoin

This endpoint returns wallet-level balance and transaction data for an extended public key or output descriptor. The indexer derives the wallet's addresses and returns the combined result.

## Parameters

| Parameter | Type    | Location | Required | Description                                                              |
| --------- | ------- | -------- | -------- | ------------------------------------------------------------------------ |
| xpub      | string  | path     | Yes      | The extended public key or supported descriptor                          |
| page      | integer | query    | No       | 1-based page index for transaction history. Default 1                    |
| pageSize  | integer | query    | No       | History items per page. Default and maximum is 1000                      |
| from      | integer | query    | No       | First block height to include when filtering history                     |
| to        | integer | query    | No       | Last block height to include when filtering history                      |
| details   | string  | query    | No       | Detail level: basic, tokens, tokenBalances, txids, or txs. Default txids |
| tokens    | string  | query    | No       | Which derived addresses to include: nonzero, used, or derived            |
| gap       | integer | query    | No       | Derivation gap limit, capped by the server                               |
| secondary | string  | query    | No       | Secondary fiat currency code for converted values                        |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz?details=tokens&tokens=used'
```
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
            "name": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
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

| Field         | Type    | Description                                                 |
| ------------- | ------- | ----------------------------------------------------------- |
| balance       | string  | Combined confirmed balance of the wallet in satoshis        |
| totalReceived | string  | Combined total received in satoshis                         |
| totalSent     | string  | Combined total sent in satoshis                             |
| txs           | integer | Number of confirmed transactions across the wallet          |
| usedTokens    | integer | Number of derived addresses that have been used             |
| tokens        | array   | Derived address rows with path, balance, and transfer count |

## Use Cases

* **Wallet Balances**: Return a full wallet balance from one xpub call
* **Address Derivation**: List derived addresses without deriving them locally
* **Portfolio Trackers**: Aggregate wallet activity across derived addresses
* **Gap Control**: Tune the derivation gap limit for sparsely used wallets
* **Receive Flows**: Find the next unused address from the derived rows

## Error Handling

| HTTP Status | Message        | Description                                      |
| ----------- | -------------- | ------------------------------------------------ |
| 400         | Bad request    | The address, XPUB, or descriptor is malformed    |
| 404         | Not found      | No indexed data exists for the requested account |
| 500         | Internal error | The indexer failed to read account data          |
