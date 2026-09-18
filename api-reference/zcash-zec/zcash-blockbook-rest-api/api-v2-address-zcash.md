---
description: >-
  Example code for the api/v2/address REST method. Complete guide on how to use the
  api/v2/address REST method in the GetBlock Web3 documentation.
---

# api/v2/address - Zcash

This endpoint returns balance and transaction data for a single transparent Zcash address. The `details` query parameter controls how much data is returned, from a balance-only summary to full transaction objects.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| address | string | path | Yes | The transparent Zcash address to query (`t1...` or `t3...`) |
| page | integer | query | No | 1-based page index for transaction history. Default 1 |
| pageSize | integer | query | No | History items per page. Default and maximum is 1000 |
| from | integer | query | No | First block height to include when filtering history |
| to | integer | query | No | Last block height to include when filtering history |
| details | string | query | No | Detail level: basic, txids, txslight, or txs. Default txids |
| secondary | string | query | No | Secondary fiat currency code for converted values |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua?page=1&pageSize=1000&details=txids'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua?page=1&pageSize=1000&details=txids'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua?page=1&pageSize=1000&details=txids')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "page": 1,
    "totalPages": 17,
    "itemsOnPage": 1000,
    "address": "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua",
    "balance": "13956944343328",
    "totalReceived": "722382862810391",
    "totalSent": "708425918467063",
    "unconfirmedBalance": "0",
    "unconfirmedTxs": 0,
    "txs": 16868,
    "txids": [
        "e5514dac030111c2485ae57401e52bb646c83d196b7d01a7298db98e1973228d",
        "abb031d74760f4ff2eeb8ef03ed207c616a791842da1a0b375f02ccad57c30af"
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| address | string | The queried address |
| balance | string | Confirmed transparent balance in zatoshis |
| totalReceived | string | Total received in zatoshis |
| totalSent | string | Total sent in zatoshis |
| unconfirmedBalance | string | Unconfirmed balance in zatoshis |
| unconfirmedTxs | integer | Number of unconfirmed transactions |
| txs | integer | Number of confirmed transactions |
| txids | array | Transaction ids, present when details is txids |
| transactions | array | Full transaction objects, present when details is txs |

{% hint style="warning" %}
Only **transparent** addresses (`t1`, `t3`) can be queried. Shielded Sapling (`zs1`) and Orchard value is encrypted to the holder's viewing key and is not indexed, so a Unified Address's shielded balance cannot be read here. For a Unified Address, query its transparent receiver, which the JSON-RPC [z_listunifiedreceivers](../zcash-json-rpc-api/z_listunifiedreceivers-zcash.md) method extracts.
{% endhint %}

## Use Cases

* **Balance Display**: Show the transparent balance of an address
* **History Pages**: Page through an address's transaction ids or full transactions
* **Payment Detection**: Detect transparent deposits by watching an address balance
* **Accounting**: Read total received and sent for bookkeeping

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
