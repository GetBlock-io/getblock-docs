---
description: >-
  Example code for the createtransaction REST method. Complete guide on how to
  use createtransaction REST method in GetBlock Web3 documentation.
---

# /wallet/createtransaction - Tron

This endpoint builds an unsigned TRX transfer transaction from a sender, recipient, and amount. The returned transaction must be signed offline and submitted with broadcasttransaction.

## Parameters

| Parameter      | Type    | Required | Description                                                 |
| -------------- | ------- | -------- | ----------------------------------------------------------- |
| owner\_address | string  | Yes      | Sender address, base58 when visible is true                 |
| to\_address    | string  | Yes      | Recipient address, base58 when visible is true              |
| amount         | integer | Yes      | Amount to transfer in SUN                                   |
| visible        | boolean | No       | Accept and return addresses in base58 format. Default false |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/wallet/createtransaction' \
--header 'Content-Type: application/json' \
--data-raw '{
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "to_address": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh",
  "amount": 1000000,
  "visible": true
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/createtransaction',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "to_address": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh", "amount": 1000000, "visible": true})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/createtransaction',
    headers={'Content-Type': 'application/json'},
    json={"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "to_address": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh", "amount": 1000000, "visible": true}
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
    "expiration": 1719400060000,
    "timestamp": 1719400000000
  },
  "raw_data_hex": "0a02..."
}
```

## Response Parameters

| Field          | Type   | Description                                    |
| -------------- | ------ | ---------------------------------------------- |
| txID           | string | The transaction id of the unsigned transaction |
| raw\_data      | object | The transaction's contract and metadata        |
| raw\_data\_hex | string | Hex serialization used as the signing payload  |

## Use Cases

* **TRX Transfers**: Build a native TRX transfer for signing
* **Offline Signing**: Produce an unsigned transaction to sign on an air-gapped device
* **Wallet Backends**: Construct transfers server-side before client signing
* **Batch Payments**: Build multiple transfers before broadcasting

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
