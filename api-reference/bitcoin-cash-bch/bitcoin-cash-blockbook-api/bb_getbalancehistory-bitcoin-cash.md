---
description: >-
  Example code for the bb_getBalanceHistory JSON-RPC method. Complete guide on
  how to use the bb_getBalanceHistory JSON-RPC method in the GetBlock Web3
  documentation.
---

# bb\_getBalanceHistory - Bitcoin Cash

This method returns aggregated balance-change history for an address, extended public key, or descriptor over a time range. Results are grouped into intervals and can include fiat rates.

## Parameters

| Parameter    | Type    | Required | Description                                                         |
| ------------ | ------- | -------- | ------------------------------------------------------------------- |
| descriptor   | string  | Yes      | Address, extended public key, or descriptor. URL-encode descriptors |
| from         | integer | No       | Unix timestamp lower bound                                          |
| to           | integer | No       | Unix timestamp upper bound                                          |
| fiatcurrency | string  | No       | Fiat currency code to include in rates                              |
| groupBy      | integer | No       | Aggregation interval in seconds. Default 3600                       |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?from=1617100000&to=1617300000&fiatcurrency=usd'
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
    "method": "bb_getBalanceHistory",
    "params": [
        "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a",
        {
            "from": 1617100000,
            "to": 1617300000,
            "fiatcurrency": "usd"
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?from=1617100000&to=1617300000&fiatcurrency=usd'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?from=1617100000&to=1617300000&fiatcurrency=usd')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "time": 1617177600,
        "txs": 1,
        "received": "9407625",
        "sent": "0",
        "sentToSelf": "0",
        "rates": {
            "usd": 512.34
        }
    }
]
```

## Response Parameters

| Field      | Type    | Description                                              |
| ---------- | ------- | -------------------------------------------------------- |
| time       | integer | Start of the interval as a Unix timestamp                |
| txs        | integer | Number of transactions in the interval                   |
| received   | string  | Total received in the interval, in satoshis              |
| sent       | string  | Total sent in the interval, in satoshis                  |
| sentToSelf | string  | Amount sent back to the same account, in satoshis        |
| rates      | object  | Fiat rates at the interval, when a currency is requested |

## Use Cases

* **Balance Charts**: Plot an address balance over time in intervals
* **Fiat Valuation**: Attach historical fiat rates to balance changes
* **Activity Windows**: Aggregate history by day, hour, or custom interval
* **Reporting**: Produce time-bucketed received and sent totals

## Error Handling

| Error Code   | Message        | Description                                      |
| ------------ | -------------- | ------------------------------------------------ |
| 400 / -32602 | Bad request    | The address, XPUB, or descriptor is malformed    |
| 404 / -32603 | Not found      | No indexed data exists for the requested account |
| 500 / -32603 | Internal error | The indexer failed to read account data          |
