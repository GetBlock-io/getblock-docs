---
description: >-
  Example code for the getcontract REST method. Complete guide on how to use
  getcontract REST method in GetBlock Web3 documentation.
---

# /wallet/getcontract - Tron

This endpoint returns the metadata and ABI of a smart contract deployed at an address.

## Parameters

| Parameter | Type    | Required | Description                                  |
| --------- | ------- | -------- | -------------------------------------------- |
| value     | string  | Yes      | The contract address                         |
| visible   | boolean | No       | Accept and return addresses in base58 format |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/wallet/getcontract' \
--header 'Content-Type: application/json' \
--data-raw '{
  "value": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t",
  "visible": true
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/getcontract',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"value": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t", "visible": true})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/getcontract',
    headers={'Content-Type': 'application/json'},
    json={"value": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t", "visible": true}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "contract_address": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t",
  "origin_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "abi": {
    "entrys": [
      {
        "name": "transfer",
        "type": "Function"
      }
    ]
  },
  "name": "TetherToken",
  "consume_user_resource_percent": 100,
  "origin_energy_limit": 10000000
}
```

## Response Parameters

| Field                            | Type    | Description                        |
| -------------------------------- | ------- | ---------------------------------- |
| contract\_address                | string  | The contract address               |
| origin\_address                  | string  | The deployer address               |
| abi                              | object  | The contract ABI, when available   |
| name                             | string  | The contract name                  |
| consume\_user\_resource\_percent | integer | Share of Energy paid by the caller |

## Use Cases

* **ABI Retrieval**: Read a contract's ABI to encode calls
* **Contract Inspection**: Read a contract's name and deployer
* **Energy Policy**: Read the caller's Energy share for the contract
* **Verification**: Confirm a contract exists at an address

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
