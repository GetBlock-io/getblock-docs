---
description: >-
  Example code for the bb_getAddress JSON-RPC method. Complete guide on how to
  use the bb_getAddress JSON-RPC method in the GetBlock Web3 documentation.
---

# bb\_getAddress - Bitcoin Cash

This method returns balance and transaction data for a single Bitcoin Cash address. The level of detail is controlled by the details parameter, from a balance-only summary to full transaction objects.

## Parameters

| Parameter | Type    | Required | Description                                                    |
| --------- | ------- | -------- | -------------------------------------------------------------- |
| address   | string  | Yes      | The Bitcoin Cash address to query                              |
| page      | integer | No       | 1-based page index for transaction history. Default 1          |
| pageSize  | integer | No       | Number of history items per page. Default and maximum is 1000  |
| from      | integer | No       | First block height to include when filtering history           |
| to        | integer | No       | Last block height to include when filtering history            |
| details   | string  | No       | Level of detail: basic, txids, txslight, or txs. Default txids |
| filter    | string  | No       | Filter history by input or output side of the address          |
| secondary | string  | No       | Secondary fiat currency code to include converted values       |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?details=txids'
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
    "method": "bb_getAddress",
    "params": [
        "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a",
        {
            "page": 1,
            "size": 1000,
            "details": "txids"
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?details=txids'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?details=txids')

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
    "address": "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a",
    "balance": "9407625",
    "totalReceived": "45231890",
    "totalSent": "35824265",
    "unconfirmedBalance": "0",
    "unconfirmedTxs": 0,
    "txs": 5,
    "txids": [
        "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
        "780791bb2d5a8ccda4b5a707967a8e15b412814852c58c77299e85579bb65587"
    ]
}
```

## Response Parameters

| Field              | Type    | Description                                                    |
| ------------------ | ------- | -------------------------------------------------------------- |
| address            | string  | The queried address                                            |
| balance            | string  | Confirmed balance in satoshis                                  |
| totalReceived      | string  | Total received in satoshis                                     |
| totalSent          | string  | Total sent in satoshis                                         |
| unconfirmedBalance | string  | Unconfirmed balance in satoshis                                |
| txs                | integer | Number of confirmed transactions                               |
| txids              | array   | Transaction ids for the address, present when details is txids |
| transactions       | array   | Full transaction objects, present when details is txs          |

## Use Cases

* **Balance Display**: Show the confirmed and unconfirmed balance of an address
* **History Pages**: Page through an address's transaction ids or full transactions
* **Payment Detection**: Detect incoming payments by watching an address balance
* **Accounting**: Read total received and sent for bookkeeping
* **Block Filtering**: Restrict history to a block-height range with from and to

## Error Handling

| Error Code   | Message        | Description                                                  |
| ------------ | -------------- | ------------------------------------------------------------ |
| 400 / -32602 | Bad request    | The address is malformed or not a valid Bitcoin Cash address |
| 404 / -32603 | Not found      | No indexed data exists for the requested address             |
| 500 / -32603 | Internal error | The indexer failed to read address data                      |
