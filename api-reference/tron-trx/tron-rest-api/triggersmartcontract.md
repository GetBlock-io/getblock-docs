# triggersmartcontract

This endpoint builds an unsigned transaction that calls a state-changing smart-contract function, such as a TRC-20 transfer. The returned transaction must be signed and broadcast.

## Parameters

| Parameter          | Type    | Required | Description                                           |
| ------------------ | ------- | -------- | ----------------------------------------------------- |
| owner\_address     | string  | Yes      | Caller address                                        |
| contract\_address  | string  | Yes      | The contract address to call                          |
| function\_selector | string  | Yes      | Function signature, such as transfer(address,uint256) |
| parameter          | string  | No       | ABI-encoded arguments, without the selector           |
| fee\_limit         | integer | No       | Maximum TRX in SUN to spend on Energy for the call    |
| visible            | boolean | No       | Accept and return addresses in base58 format          |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/wallet/triggersmartcontract' \
--header 'Content-Type: application/json' \
--data-raw '{
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "contract_address": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t",
  "function_selector": "transfer(address,uint256)",
  "parameter": "0000000000000000000000004142b5e01c8c59a25d78acdbec2bfc7e89e5e86300000000000000000000000000000000000000000000000000000000000f4240",
  "fee_limit": 100000000,
  "visible": true
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/triggersmartcontract',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "contract_address": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t", "function_selector": "transfer(address,uint256)", "parameter": "0000000000000000000000004142b5e01c8c59a25d78acdbec2bfc7e89e5e86300000000000000000000000000000000000000000000000000000000000f4240", "fee_limit": 100000000, "visible": true})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/wallet/triggersmartcontract',
    headers={'Content-Type': 'application/json'},
    json={"owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "contract_address": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t", "function_selector": "transfer(address,uint256)", "parameter": "0000000000000000000000004142b5e01c8c59a25d78acdbec2bfc7e89e5e86300000000000000000000000000000000000000000000000000000000000f4240", "fee_limit": 100000000, "visible": true}
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
  "transaction": {
    "txID": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62",
    "raw_data": {
      "contract": [
        {
          "type": "TriggerSmartContract"
        }
      ]
    },
    "raw_data_hex": "0a02..."
  }
}
```

## Response Parameters

| Field                      | Type    | Description                                      |
| -------------------------- | ------- | ------------------------------------------------ |
| result.result              | boolean | true when the transaction was built successfully |
| transaction.txID           | string  | The transaction id of the unsigned call          |
| transaction.raw\_data\_hex | string  | Hex serialization used as the signing payload    |

## Use Cases

* **Token Transfers**: Build a TRC-20 transfer such as USDT
* **Contract Writes**: Call any state-changing contract function
* **dApp Actions**: Build swaps, approvals, and mints for signing
* **Fee Control**: Cap Energy spend with fee\_limit

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
