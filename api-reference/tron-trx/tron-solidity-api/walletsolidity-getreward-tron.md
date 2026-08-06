---
description: >-
  Example code for the getreward Solidity API method. Complete guide on how to
  use getreward Solidity API method in GetBlock Web3 documentation.
---

# /walletsolidity/getreward - Tron

This endpoint returns the unclaimed voting and staking rewards for an account, read from confirmed state on the Solidity node.

{% hint style="info" %}
This is a Solidity-node endpoint. It returns only confirmed, irreversible data, so it is the correct interface for balance and payment verification. The Fullnode serves the same operation at `https://go.getblock.io/<ACCESS-TOKEN>/wallet/getreward` over the latest, possibly unconfirmed state.
{% endhint %}

## Parameters

| Parameter | Type    | Required | Description                                  |
| --------- | ------- | -------- | -------------------------------------------- |
| address   | string  | Yes      | The account address                          |
| visible   | boolean | No       | Accept and return addresses in base58 format |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getreward' \
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
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getreward',
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
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getreward',
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
  "reward": 4521000
}
```

## Response Parameters

| Field  | Type    | Description                               |
| ------ | ------- | ----------------------------------------- |
| reward | integer | Unclaimed rewards for the account, in SUN |

## Use Cases

* **Rewards Display**: Show an account's confirmed unclaimed rewards
* **Claim Flows**: Read the rewards balance before building a withdrawal
* **Staking Dashboards**: Track reward accrual over time
* **Reconciliation**: Confirm reward balances for accounting

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
