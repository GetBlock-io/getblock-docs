---
description: >-
  Example code for the getaccount Solidity API method. Complete guide on how to
  use getaccount Solidity API method in GetBlock Web3 documentation.
---

# /walletsolidity/getaccount - Tron

This endpoint returns the on-chain data for an account, including its TRX balance in SUN, permissions, TRC-10 asset balances, and staking state.

{% hint style="info" %}
This is a Solidity-node endpoint. It returns only confirmed, irreversible data, so it is the correct interface for balance and payment verification. The Fullnode serves the same operation at `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getaccount` over the latest, possibly unconfirmed state.
{% endhint %}

## Parameters

| Parameter | Type    | Required | Description                                                        |
| --------- | ------- | -------- | ------------------------------------------------------------------ |
| address   | string  | Yes      | Account address, base58 (T...) when visible is true, otherwise hex |
| visible   | boolean | No       | Return and accept addresses in base58 format. Default false        |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/getaccount' \
--header 'Content-Type: application/json' \
--data-raw '{
  "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "visible": true
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/getaccount',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "visible": true})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/getaccount',
    headers={'Content-Type': 'application/json'},
    json={"address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "visible": true}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "balance": 85623170,
  "create_time": 1675582485000,
  "account_resource": {
    "energy_window_size": 28800000
  },
  "owner_permission": {
    "permission_name": "owner",
    "threshold": 1
  },
  "frozenV2": [
    {
      "type": "ENERGY"
    }
  ],
  "assetV2": [
    {
      "key": "1004977",
      "value": 8888880000
    }
  ]
}
```

## Response Parameters

| Field             | Type    | Description                                   |
| ----------------- | ------- | --------------------------------------------- |
| address           | string  | The queried account address                   |
| balance           | integer | TRX balance in SUN (1 TRX = 1,000,000 SUN)    |
| frozenV2          | array   | Stake 2.0 staking positions by resource type  |
| account\_resource | object  | Energy-related resource state for the account |
| assetV2           | array   | TRC-10 asset balances held by the account     |

## Use Cases

* **Balance Display**: Show an account's TRX balance and assets
* **Staking State**: Read an account's Stake 2.0 positions
* **Permission Checks**: Read an account's owner and active permissions
* **Account Activation**: Detect whether an address has been activated on-chain

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
