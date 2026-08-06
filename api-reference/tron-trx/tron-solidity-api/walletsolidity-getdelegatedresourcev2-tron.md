---
description: >-
  Example code for the getdelegatedresourcev2 Solidity API method. Complete
  guide on how to use getdelegatedresourcev2 Solidity API method in GetBlock
  Web3 documentation.
---

# /walletsolidity/getdelegatedresourcev2 - Tron

This endpoint returns the Stake 2.0 resources delegated from one account to another, read from confirmed state on the Solidity node.

{% hint style="info" %}
This is a Solidity-node endpoint. It returns only confirmed, irreversible data, so it is the correct interface for balance and payment verification. The Fullnode serves the same operation at `https://go.getblock.io/<ACCESS-TOKEN>/wallet/getdelegatedresourcev2` over the latest, possibly unconfirmed state.
{% endhint %}

## Parameters

| Parameter   | Type    | Required | Description                                  |
| ----------- | ------- | -------- | -------------------------------------------- |
| fromAddress | string  | Yes      | Address that delegated the resource          |
| toAddress   | string  | Yes      | Address that received the delegated resource |
| visible     | boolean | No       | Accept and return addresses in base58 format |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getdelegatedresourcev2' \
--header 'Content-Type: application/json' \
--data-raw '{
  "fromAddress": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "toAddress": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh",
  "visible": true
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getdelegatedresourcev2',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"fromAddress": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "toAddress": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh", "visible": true})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getdelegatedresourcev2',
    headers={'Content-Type': 'application/json'},
    json={"fromAddress": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "toAddress": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh", "visible": true}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "delegatedResource": [
    {
      "from": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
      "to": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh",
      "frozen_balance_for_energy": 1000000000,
      "expire_time_for_energy": 0
    }
  ]
}
```

## Response Parameters

| Field                                             | Type    | Description                                 |
| ------------------------------------------------- | ------- | ------------------------------------------- |
| delegatedResource                                 | array   | Delegation entries between the two accounts |
| delegatedResource\[].frozen\_balance\_for\_energy | integer | Staked TRX delegated for Energy, in SUN     |
| delegatedResource\[].expire\_time\_for\_energy    | integer | Lock expiry time, or 0 when not locked      |

## Use Cases

* **Delegation Audits**: Read confirmed resource delegations between accounts
* **Sponsorship Tracking**: Verify Energy delegated to a receiver
* **Reconciliation**: Confirm delegation state for accounting
* **Wallet Views**: Show a user their confirmed delegations

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
