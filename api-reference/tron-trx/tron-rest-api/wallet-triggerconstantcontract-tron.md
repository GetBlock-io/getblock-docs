---
description: >-
  Example code for the triggerconstantcontract REST method. Complete guide on
  how to use triggerconstantcontract REST method in GetBlock Web3 documentation.
---

# /wallet/triggerconstantcontract - Tron

This endpoint executes a read-only smart-contract call locally on the node, without creating a transaction. It is the primary method for reading contract state, such as a TRC-20 token balance, and returns the result in `constant_result`.

## Parameters

| Parameter          | Type    | Required | Description                                    |
| ------------------ | ------- | -------- | ---------------------------------------------- |
| owner\_address     | string  | Yes      | Caller address                                 |
| contract\_address  | string  | Yes      | The contract address to call                   |
| function\_selector | string  | Yes      | Function signature, such as balanceOf(address) |
| parameter          | string  | No       | ABI-encoded arguments, without the selector    |
| visible            | boolean | No       | Accept and return addresses in base58 format   |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/wallet/triggerconstantcontract' \
--header 'Content-Type: application/json' \
--data-raw '{
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "contract_address": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t",
  "function_selector": "balanceOf(address)",
  "parameter": "0000000000000000000000004142b5e01c8c59a25d78acdbec2bfc7e89e5e863",
  "visible": true
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/triggerconstantcontract',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "contract_address": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t", "function_selector": "balanceOf(address)", "parameter": "0000000000000000000000004142b5e01c8c59a25d78acdbec2bfc7e89e5e863", "visible": true})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/triggerconstantcontract',
    headers={'Content-Type': 'application/json'},
    json={"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "contract_address": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t", "function_selector": "balanceOf(address)", "parameter": "0000000000000000000000004142b5e01c8c59a25d78acdbec2bfc7e89e5e863", "visible": true}
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
  "constant_result": [
    "00000000000000000000000000000000000000000000000000000000000f4240"
  ],
  "energy_used": 1082
}
```

## Response Parameters

| Field            | Type    | Description                                           |
| ---------------- | ------- | ----------------------------------------------------- |
| result.result    | boolean | true when the call executed successfully              |
| constant\_result | array   | ABI-encoded return data of the call, hex-encoded      |
| energy\_used     | integer | Energy the call would consume if run as a transaction |

## Use Cases

* **Token Balances**: Read a TRC-20 balance with balanceOf
* **Contract State**: Read view and pure functions without a transaction
* **Price Feeds**: Query on-chain oracle values
* **Energy Estimation**: Read energy\_used to size a later transaction

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
