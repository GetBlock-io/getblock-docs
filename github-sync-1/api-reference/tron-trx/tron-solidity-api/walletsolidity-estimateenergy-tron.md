---
description: >-
  Example code for the estimateenergy Solidity API method. Complete guide on how
  to use estimateenergy Solidity API method in GetBlock Web3 documentation.
---

# /walletsolidity/estimateenergy - Tron

This endpoint estimates the Energy required to execute a smart-contract call, without submitting a transaction. It is used to set a fee limit before a call.

{% hint style="info" %}
This is a Solidity-node endpoint. It returns only confirmed, irreversible data, so it is the correct interface for balance and payment verification. The Fullnode serves the same operation at `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/estimateenergy` over the latest, possibly unconfirmed state.
{% endhint %}

## Parameters

| Parameter          | Type    | Required | Description                                  |
| ------------------ | ------- | -------- | -------------------------------------------- |
| owner\_address     | string  | Yes      | Caller address                               |
| contract\_address  | string  | Yes      | The contract address to call                 |
| function\_selector | string  | Yes      | Function signature to estimate               |
| parameter          | string  | No       | ABI-encoded arguments, without the selector  |
| visible            | boolean | No       | Accept and return addresses in base58 format |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/estimateenergy' \
--header 'Content-Type: application/json' \
--data-raw '{
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "contract_address": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t",
  "function_selector": "transfer(address,uint256)",
  "parameter": "0000000000000000000000004142b5e01c8c59a25d78acdbec2bfc7e89e5e86300000000000000000000000000000000000000000000000000000000000f4240",
  "visible": true
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/estimateenergy',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "contract_address": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t", "function_selector": "transfer(address,uint256)", "parameter": "0000000000000000000000004142b5e01c8c59a25d78acdbec2bfc7e89e5e86300000000000000000000000000000000000000000000000000000000000f4240", "visible": true})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/estimateenergy',
    headers={'Content-Type': 'application/json'},
    json={"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "contract_address": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t", "function_selector": "transfer(address,uint256)", "parameter": "0000000000000000000000004142b5e01c8c59a25d78acdbec2bfc7e89e5e86300000000000000000000000000000000000000000000000000000000000f4240", "visible": true}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "result": {
    "result": true
  },
  "energy_required": 64285
}
```

## Response Parameters

| Field            | Type    | Description                            |
| ---------------- | ------- | -------------------------------------- |
| result.result    | boolean | true when the estimate succeeded       |
| energy\_required | integer | Estimated Energy the call will consume |

## Use Cases

* **Fee Limits**: Set fee\_limit from the estimated Energy
* **Cost Preview**: Show a user the Energy cost of an action
* **Preflight**: Detect a call that would fail before submitting
* **Batch Sizing**: Sum estimates across a batch of calls

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
