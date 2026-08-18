---
description: >-
  Example code for the evaluateTransaction JSON-RPC method. Complete guide on
  how to use evaluateTransaction JSON-RPC in GetBlock Web3 documentation.
---

# evaluateTransaction - Cardano

This method evaluates the execution units required by the Plutus scripts in a transaction, without submitting it. It returns the memory and CPU budget each script requires.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| transaction    | object | Yes      | Object with a cbor field holding the base16 transaction |
| additionalUtxo | array  | No       | Extra UTXOs to resolve inputs not yet on-chain          |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "evaluateTransaction",
    "params": {
        "transaction": {
            "cbor": "84a300818258200e...transaction-cbor-hex...ff"
        },
        "additionalUtxo": []
    },
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"jsonrpc": "2.0", "method": "evaluateTransaction", "params": {"transaction": {"cbor": "84a300818258200e...transaction-cbor-hex...ff"}, "additionalUtxo": []}, "id": "getblock.io"})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={"jsonrpc": "2.0", "method": "evaluateTransaction", "params": {"transaction": {"cbor": "84a300818258200e...transaction-cbor-hex...ff"}, "additionalUtxo": []}, "id": "getblock.io"}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "method": "evaluateTransaction",
    "result": [
        {
            "validator": {
                "index": 0,
                "purpose": "spend"
            },
            "budget": {
                "memory": 1700000,
                "cpu": 476468950
            }
        }
    ],
    "id": "getblock.io"
}
```

## Response Parameters

| Field     | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| validator | object | The redeemer's index and purpose        |
| budget    | object | Required memory and CPU execution units |

## Use Cases

* **Script Budgeting**: Measure execution units before submitting
* **Fee Estimation**: Price script execution into the transaction fee
* **dApp Testing**: Validate that scripts stay within limits
* **Transaction Building**: Set redeemer budgets from measured units

## Error Handling

| Error Code | Message                          | Description                                   |
| ---------- | -------------------------------- | --------------------------------------------- |
| -32602     | Invalid params                   | The transaction CBOR is missing or malformed  |
| 3010       | Cannot create evaluation context | The additional UTXO set could not be resolved |
| 3011       | Script execution failure         | One or more scripts failed during evaluation  |
| -32603     | Internal error                   | The node failed to evaluate the transaction   |
