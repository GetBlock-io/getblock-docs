---
description: >-
  Example code for the gettransactioncountbyblocknum Solidity API method.
  Complete guide on how to use gettransactioncountbyblocknum Solidity API method
  in GetBlock Web3 documentation.
---

# /walletsolidity/gettransactioncountbyblocknum - Tron

This endpoint returns the number of transactions in a confirmed block, identified by height.

{% hint style="info" %}
This is a Solidity-node endpoint. It returns only confirmed, irreversible data, so it is the correct interface for balance and payment verification. The Fullnode serves the same operation at `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/gettransactioncountbyblocknum` over the latest, possibly unconfirmed state.
{% endhint %}

## Parameters

| Parameter | Type    | Required | Description      |
| --------- | ------- | -------- | ---------------- |
| num       | integer | Yes      | The block height |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactioncountbyblocknum' \
--header 'Content-Type: application/json' \
--data-raw '{
  "num": 68000000
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactioncountbyblocknum',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"num": 68000000})
    }
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactioncountbyblocknum',
    headers={'Content-Type': 'application/json'},
    json={"num": 68000000}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "count": 312
}
```

## Response Parameters

| Field | Type    | Description                                   |
| ----- | ------- | --------------------------------------------- |
| count | integer | Number of transactions in the confirmed block |

## Use Cases

* **Block Analytics**: Track confirmed transactions per block
* **Pagination**: Size iteration over a confirmed block's transactions
* **Throughput Metrics**: Measure finalized block fullness
* **Explorer Headers**: Show the transaction count on a block page

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
