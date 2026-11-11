---
description: >-
  Example code for the freezebalancev2 REST method. Complete guide on how to use
  freezebalancev2 REST method in GetBlock Web3 documentation.
---

# /wallet/freezebalancev2 - Tron

This endpoint builds an unsigned Stake 2.0 transaction that stakes TRX to gain Energy or Bandwidth. The returned transaction must be signed and broadcast.

## Parameters

| Parameter       | Type    | Required | Description                                  |
| --------------- | ------- | -------- | -------------------------------------------- |
| owner\_address  | string  | Yes      | Staking account address                      |
| frozen\_balance | integer | Yes      | Amount of TRX to stake, in SUN               |
| resource        | string  | Yes      | Resource to gain: ENERGY or BANDWIDTH        |
| visible         | boolean | No       | Accept and return addresses in base58 format |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/freezebalancev2' \
--header 'Content-Type: application/json' \
--data-raw '{
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "frozen_balance": 1000000000,
  "resource": "ENERGY",
  "visible": true
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/freezebalancev2',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "frozen_balance": 1000000000, "resource": "ENERGY", "visible": true})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/freezebalancev2',
    headers={'Content-Type': 'application/json'},
    json={"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "frozen_balance": 1000000000, "resource": "ENERGY", "visible": true}
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
        "type": "FreezeBalanceV2Contract"
      }
    ]
  },
  "raw_data_hex": "0a02..."
}
```

## Response Parameters

| Field          | Type   | Description                                            |
| -------------- | ------ | ------------------------------------------------------ |
| txID           | string | The transaction id of the unsigned staking transaction |
| raw\_data      | object | The FreezeBalanceV2Contract and metadata               |
| raw\_data\_hex | string | Hex serialization used as the signing payload          |

## Use Cases

* **Gain Energy**: Stake TRX to obtain Energy for contract calls
* **Gain Bandwidth**: Stake TRX to obtain Bandwidth for transactions
* **Fee Reduction**: Reduce burned TRX by staking for resources
* **Validator Support**: Acquire TRON Power for voting through staking

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
