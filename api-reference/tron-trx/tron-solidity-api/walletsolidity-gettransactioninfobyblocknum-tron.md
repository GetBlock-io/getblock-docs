---
description: >-
  Example code for the gettransactioninfobyblocknum Solidity API method.
  Complete guide on how to use gettransactioninfobyblocknum Solidity API method
  in GetBlock Web3 documentation.
---

# /walletsolidity/gettransactioninfobyblocknum - Tron

This endpoint returns the execution results of all transactions in a confirmed block, identified by height. It is used to read every receipt and log in a block at once.

{% hint style="info" %}
This is a Solidity-node endpoint. It returns only confirmed, irreversible data, so it is the correct interface for balance and payment verification. The Fullnode serves the same operation at `https://go.getblock.io/<ACCESS-TOKEN>/wallet/gettransactioninfobyblocknum` over the latest, possibly unconfirmed state.
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
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactioninfobyblocknum' \
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
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactioninfobyblocknum',
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
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactioninfobyblocknum',
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
[
  {
    "id": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62",
    "blockNumber": 68000000,
    "blockTimeStamp": 1719400000000,
    "receipt": {
      "energy_usage_total": 65000,
      "net_usage": 345,
      "result": "SUCCESS"
    },
    "fee": 27000,
    "log": [
      {
        "address": "a614f803b6fd780986a42c78ec9c7f77e6ded13c",
        "topics": [
          "ddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
        ],
        "data": "00000000000000000000000000000000000000000000000000000000000f4240"
      }
    ]
  }
]
```

## Response Parameters

| Field       | Type    | Description                                         |
| ----------- | ------- | --------------------------------------------------- |
| id          | string  | Transaction id                                      |
| blockNumber | integer | Block height                                        |
| receipt     | object  | Energy and Bandwidth used, and the execution result |
| log         | array   | Smart-contract event logs emitted in the block      |

## Use Cases

* **Block Receipts**: Read every confirmed receipt in a block in one call
* **TRC-20 Indexing**: Extract all token Transfer logs for a block
* **Settlement Reconciliation**: Confirm outcomes of a whole confirmed block
* **Analytics**: Aggregate energy and fees across a block

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
