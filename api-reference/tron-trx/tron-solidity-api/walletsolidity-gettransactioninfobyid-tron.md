---
description: >-
  Example code for the gettransactioninfobyid Solidity API method. Complete
  guide on how to use gettransactioninfobyid Solidity API method in GetBlock
  Web3 documentation.
---

# /walletsolidity/gettransactioninfobyid - Tron

This endpoint returns the execution result of a transaction: its block, Energy and Bandwidth used, fee, and any smart-contract logs. This is the TRON equivalent of a transaction receipt.

{% hint style="info" %}
This is a Solidity-node endpoint. It returns only confirmed, irreversible data, so it is the correct interface for balance and payment verification. The Fullnode serves the same operation at `https://go.getblock.io/<ACCESS-TOKEN>/wallet/gettransactioninfobyid` over the latest, possibly unconfirmed state.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description               |
| --------- | ------ | -------- | ------------------------- |
| value     | string | Yes      | The transaction id (hash) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactioninfobyid' \
--header 'Content-Type: application/json' \
--data-raw '{
  "value": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactioninfobyid',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"value": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactioninfobyid',
    headers={'Content-Type': 'application/json'},
    json={"value": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
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
  "contractResult": [
    "..."
  ],
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
```

## Response Parameters

| Field                        | Type    | Description                                          |
| ---------------------------- | ------- | ---------------------------------------------------- |
| id                           | string  | The transaction id                                   |
| blockNumber                  | integer | Block height the transaction was included in         |
| receipt.energy\_usage\_total | integer | Total Energy consumed                                |
| receipt.net\_usage           | integer | Bandwidth consumed                                   |
| receipt.result               | string  | Execution result, such as SUCCESS or REVERT          |
| log                          | array   | Smart-contract event logs emitted by the transaction |

## Use Cases

* **Receipt Reads**: Confirm a transaction succeeded and read its cost
* **TRC-20 Events**: Read Transfer logs emitted by a token contract
* **Energy Accounting**: Read Energy and Bandwidth consumed by a call
* **Fee Reconciliation**: Read the fee burned for a transaction

## Error Handling

| HTTP Status | Message        | Description                                                          |
| ----------- | -------------- | -------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; a missing transaction returns an empty object |
| 400         | Bad request    | The transaction id or body is malformed                              |
| 500         | Internal error | The node failed to read the transaction                              |
