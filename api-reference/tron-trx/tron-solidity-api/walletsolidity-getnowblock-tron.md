---
description: >-
  Example code for the getnowblock Solidity API method. Complete guide on how to
  use getnowblock Solidity API method in GetBlock Web3 documentation.
---

# /walletsolidity/getnowblock - Tron

This endpoint returns the most recent block on the chain, including its header and transactions.

{% hint style="info" %}
This is a Solidity-node endpoint. It returns only confirmed, irreversible data, so it is the correct interface for balance and payment verification. The Fullnode serves the same operation at `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getnowblock` over the latest, possibly unconfirmed state.
{% endhint %}

## Parameters

This endpoint does not require parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/getnowblock' \
--header 'Content-Type: application/json' \
--data-raw '{}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/getnowblock',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/getnowblock',
    headers={'Content-Type': 'application/json'},
    json={}
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
      "txTrieRoot": "434275024b822da93ed95839388888add6734518b482c21e7add1e6c384b866e",
      "witness_address": "41e72d833e0c46837c0802864acc5f119a0a904d05",
      "parentHash": "0000000002f3a5af...",
      "timestamp": 1719400000000
    },
    "witness_signature": "e4971f53..."
  },
  "transactions": [
    {
      "txID": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"
    }
  ]
}
```

## Response Parameters

| Field                                    | Type    | Description                                  |
| ---------------------------------------- | ------- | -------------------------------------------- |
| blockID                                  | string  | The block hash                               |
| block\_header.raw\_data.number           | integer | Block height                                 |
| block\_header.raw\_data.timestamp        | integer | Block time in milliseconds                   |
| block\_header.raw\_data.witness\_address | string  | Super Representative that produced the block |
| transactions                             | array   | Transactions included in the block           |

## Use Cases

* **Tip Tracking**: Read the current block height and hash
* **Sync Checks**: Compare the tip against a local index
* **Timestamps**: Read the latest block time
* **Polling**: Detect new blocks by watching the height

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
