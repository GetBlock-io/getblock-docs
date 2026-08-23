# bb\_getxpub bitcoin

This method returns wallet-level balance and transaction data for an extended public key or output descriptor. The indexer derives the wallet's addresses and returns the combined result.

## Parameters

| Parameter | Type   | Required | Description                                                |
| --------- | ------ | -------- | ---------------------------------------------------------- |
| xpub      | string | Yes      | Extended public key or output descriptor                   |
| details   | string | No       | Level of detail: basic, tokens, txids, txs. Default txids  |
| tokens    | string | No       | Which derived addresses to include: nonzero, used, derived |

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
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/xpub/xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz?details=tokens&tokens=used');
const data = await response.json();
console.log(data);
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
  "balance": "275000000",
  "totalReceived": "900000000",
  "totalSent": "625000000",
  "txs": 128,
  "usedTokens": 24,
  "tokens": [
    {
      "type": "XPUBAddress",
      "name": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
      "path": "m/84'/0'/0'/0/0",
      "transfers": 12,
      "balance": "50000000"
    }
  ]
}
```

## Response Parameters

| Field         | Type    | Description                         |
| ------------- | ------- | ----------------------------------- |
| balance       | string  | Combined wallet balance in satoshis |
| totalReceived | string  | Total received across the wallet    |
| txs           | integer | Number of wallet transactions       |
| usedTokens    | integer | Number of used derived addresses    |
| tokens        | array   | Per-address derivation data         |

## Use Cases

* **Wallet Balances**: Read a whole wallet from one xpub
* **Address Discovery**: Enumerate used derived addresses
* **Portfolio Tools**: Aggregate wallet activity
* **Accounting**: Reconcile wallet-level totals

## Error Handling

| HTTP Status | Message      | Description                               |
| ----------- | ------------ | ----------------------------------------- |
| 400         | Bad request  | A path or query parameter is malformed    |
| 403         | Forbidden    | Missing or invalid ACCESS-TOKEN           |
| 404         | Not found    | The requested resource does not exist     |
| 500         | Server error | The indexer failed to process the request |
