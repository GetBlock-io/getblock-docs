# gettransactionbyid

This endpoint returns a transaction by its id, including its contract data, signatures, and raw data.

{% hint style="info" %}
This is a Solidity-node endpoint. It returns only confirmed, irreversible data, so it is the correct interface for balance and payment verification. The Fullnode serves the same operation at `https://go.getblock.io/<ACCESS-TOKEN>/wallet/gettransactionbyid` over the latest, possibly unconfirmed state.
{% endhint %}

## Parameters

| Parameter | Type    | Required | Description                                      |
| --------- | ------- | -------- | ------------------------------------------------ |
| value     | string  | Yes      | The transaction id (hash)                        |
| visible   | boolean | No       | Return addresses in base58 format. Default false |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactionbyid' \
--header 'Content-Type: application/json' \
--data-raw '{
  "value": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62",
  "visible": true
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactionbyid',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"value": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62", "visible": true})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactionbyid',
    headers={'Content-Type': 'application/json'},
    json={"value": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62", "visible": true}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "txID": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62",
  "raw_data": {
    "contract": [
      {
        "type": "TransferContract",
        "parameter": {
          "value": {
            "amount": 1000000,
            "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
            "to_address": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh"
          }
        }
      }
    ],
    "timestamp": 1719400000000
  },
  "signature": [
    "a1b2c3..."
  ],
  "ret": [
    {
      "contractRet": "SUCCESS"
    }
  ]
}
```

## Response Parameters

| Field              | Type   | Description                                                     |
| ------------------ | ------ | --------------------------------------------------------------- |
| txID               | string | The transaction id                                              |
| raw\_data.contract | array  | The contract(s) carried by the transaction and their parameters |
| signature          | array  | Signatures over the transaction                                 |
| ret                | array  | Transaction result, including the contract return code          |

## Use Cases

* **Transaction Views**: Render a transaction's contract and result
* **Payment Confirmation**: Read a transfer's amount and parties
* **Signature Inspection**: Read the signatures on a transaction
* **Debugging**: Inspect the raw contract data of a transaction

## Error Handling

| HTTP Status | Message        | Description                                                          |
| ----------- | -------------- | -------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; a missing transaction returns an empty object |
| 400         | Bad request    | The transaction id or body is malformed                              |
| 500         | Internal error | The node failed to read the transaction                              |
