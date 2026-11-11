---
description: >-
  Example code for the broadcasttransaction REST method. Complete guide on how
  to use broadcasttransaction REST method in GetBlock Web3 documentation.
---

# /wallet/broadcasttransaction - Tron

This endpoint submits a signed transaction to the network. It accepts the full signed transaction object and returns whether it was accepted.

## Parameters

| Parameter   | Type   | Required | Description                                                       |
| ----------- | ------ | -------- | ----------------------------------------------------------------- |
| transaction | object | Yes      | The full signed transaction object, including the signature array |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/broadcasttransaction' \
--header 'Content-Type: application/json' \
--data-raw '{
  "txID": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62",
  "raw_data": {
    "contract": []
  },
  "raw_data_hex": "0a02...",
  "signature": [
    "a1b2c3...signature"
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/broadcasttransaction',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"txID": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62", "raw_data": {"contract": []}, "raw_data_hex": "0a02...", "signature": ["a1b2c3...signature"]})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/broadcasttransaction',
    headers={'Content-Type': 'application/json'},
    json={"txID": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62", "raw_data": {"contract": []}, "raw_data_hex": "0a02...", "signature": ["a1b2c3...signature"]}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "result": true,
  "txid": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"
}
```

## Response Parameters

| Field   | Type    | Description                                            |
| ------- | ------- | ------------------------------------------------------ |
| result  | boolean | true when the transaction is accepted into the mempool |
| txid    | string  | The transaction id, on acceptance                      |
| code    | string  | Rejection code, when the transaction is not accepted   |
| message | string  | Rejection reason, when the transaction is not accepted |

## Use Cases

* **Payment Broadcast**: Submit a signed TRX or token transfer
* **Contract Calls**: Broadcast a signed triggersmartcontract transaction
* **dApp Backends**: Relay transactions signed on the client
* **Retry Flows**: Resubmit a signed transaction from stored data

## Error Handling

| HTTP Status | Message        | Description                                                           |
| ----------- | -------------- | --------------------------------------------------------------------- |
| 200         | OK             | Returns result true on acceptance, or a code and message on rejection |
| 400         | Bad request    | The signed transaction is malformed                                   |
| 500         | Internal error | The node failed to broadcast the transaction                          |
