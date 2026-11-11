---
description: >-
  Example code for the getblockbylatestnum Solidity API method. Complete guide
  on how to use getblockbylatestnum Solidity API method in GetBlock Web3
  documentation.
---

# /walletsolidity/getblockbylatestnum - Tron

This endpoint returns the most recent confirmed blocks, up to the requested count, from the Solidity node.

{% hint style="info" %}
This is a Solidity-node endpoint. It returns only confirmed, irreversible data, so it is the correct interface for balance and payment verification. The Fullnode serves the same operation at `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getblockbylatestnum` over the latest, possibly unconfirmed state.
{% endhint %}

## Parameters

| Parameter | Type    | Required | Description                            |
| --------- | ------- | -------- | -------------------------------------- |
| num       | integer | Yes      | Number of most recent blocks to return |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/getblockbylatestnum' \
--header 'Content-Type: application/json' \
--data-raw '{
  "num": 5
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/getblockbylatestnum',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"num": 5})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/getblockbylatestnum',
    headers={'Content-Type': 'application/json'},
    json={"num": 5}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "block": [
    {
      "blockID": "0000000002f3a5b0f6d2e6c9a1b4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8",
      "block_header": {
        "raw_data": {
          "number": 68000000,
          "timestamp": 1719400000000
        }
      }
    }
  ]
}
```

## Response Parameters

| Field | Type  | Description                                                         |
| ----- | ----- | ------------------------------------------------------------------- |
| block | array | The most recent confirmed blocks, each with header and transactions |

## Use Cases

* **Recent Confirmed Blocks**: Read the latest irreversible blocks
* **Tail Indexing**: Ingest the most recent confirmed blocks
* **Dashboards**: Display recent finalized chain activity
* **Settlement**: Read confirmed blocks for payment finality

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
