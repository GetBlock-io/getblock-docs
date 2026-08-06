---
description: >-
  Example code for the unfreezebalancev2 REST method. Complete guide on how to
  use unfreezebalancev2 REST method in GetBlock Web3 documentation.
---

# /wallet/unfreezebalancev2 - Tron

This endpoint builds an unsigned Stake 2.0 transaction that unstakes previously staked TRX. After an unstake, the TRX enters a waiting period before it can be withdrawn.

## Parameters

| Parameter         | Type    | Required | Description                                  |
| ----------------- | ------- | -------- | -------------------------------------------- |
| owner\_address    | string  | Yes      | Staking account address                      |
| unfreeze\_balance | integer | Yes      | Amount of staked TRX to unstake, in SUN      |
| resource          | string  | Yes      | Resource to release: ENERGY or BANDWIDTH     |
| visible           | boolean | No       | Accept and return addresses in base58 format |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/wallet/unfreezebalancev2' \
--header 'Content-Type: application/json' \
--data-raw '{
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "unfreeze_balance": 1000000000,
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
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/unfreezebalancev2',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "unfreeze_balance": 1000000000, "resource": "ENERGY", "visible": true})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/unfreezebalancev2',
    headers={'Content-Type': 'application/json'},
    json={"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "unfreeze_balance": 1000000000, "resource": "ENERGY", "visible": true}
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
        "type": "UnfreezeBalanceV2Contract"
      }
    ]
  },
  "raw_data_hex": "0a02..."
}
```

## Response Parameters

| Field          | Type   | Description                                              |
| -------------- | ------ | -------------------------------------------------------- |
| txID           | string | The transaction id of the unsigned unstaking transaction |
| raw\_data      | object | The UnfreezeBalanceV2Contract and metadata               |
| raw\_data\_hex | string | Hex serialization used as the signing payload            |

## Use Cases

* **Release Stake**: Begin unstaking TRX previously staked for resources
* **Liquidity Recovery**: Free staked TRX after resource needs drop
* **Resource Rebalance**: Move stake between Energy and Bandwidth
* **Wallet Flows**: Build an unstake for user-initiated withdrawals

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
