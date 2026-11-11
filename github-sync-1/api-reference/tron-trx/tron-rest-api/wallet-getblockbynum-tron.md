---
description: >-
  Example code for the getblockbynum REST method. Complete guide on how to use
  getblockbynum REST method in GetBlock Web3 documentation.
---

# /wallet/getblockbynum - Tron

This endpoint returns a block by its height, including its header and transactions.

{% hint style="info" %}
This is a read endpoint. It is also served by the Solidity node at `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/getblockbynum`, which returns only confirmed, irreversible data. Use the Solidity node for balance and payment verification.
{% endhint %}

## Parameters

| Parameter | Type    | Required | Description                  |
| --------- | ------- | -------- | ---------------------------- |
| num       | integer | Yes      | The block height to retrieve |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getblockbynum' \
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getblockbynum',
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getblockbynum',
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
  "blockID": "0000000002f3a5b0f6d2e6c9a1b4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8",
  "block_header": {
    "raw_data": {
      "number": 68000000,
      "timestamp": 1719400000000,
      "witness_address": "41e72d833e0c46837c0802864acc5f119a0a904d05"
    }
  },
  "transactions": [
    {
      "txID": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"
    }
  ]
}
```

## Response Parameters

| Field                             | Type    | Description                        |
| --------------------------------- | ------- | ---------------------------------- |
| blockID                           | string  | The block hash                     |
| block\_header.raw\_data.number    | integer | Block height                       |
| block\_header.raw\_data.timestamp | integer | Block time in milliseconds         |
| transactions                      | array   | Transactions included in the block |

## Use Cases

* **Block Explorers**: Render a block by height
* **Chain Indexing**: Stream blocks by height into a store
* **USDT Tracking**: Scan a block's transactions for TRC-20 transfers
* **Confirmation Context**: Read a block's time and producer

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
