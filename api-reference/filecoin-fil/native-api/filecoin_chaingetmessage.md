---
description: >-
  Example code for the Filecoin.ChainGetMessage JSON RPC method. Complete guide
  on how to use Filecoin.ChainGetMessage JSON RPC in GetBlock Web3
  documentation.
---

# Filecoin.ChainGetMessage - Filecoin

This method returns a single on-chain message by its CID, including the sender, recipient, value in attoFIL, gas parameters, and the actor method invoked.

## Parameters

| Parameter | Type   | Required | Description                            |
| --------- | ------ | -------- | -------------------------------------- |
| cid       | object | Yes      | Message CID, as an object with a / key |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.ChainGetMessage", "params": [{"/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"}], "id": "getblock.io"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        jsonrpc: '2.0',
        method: 'Filecoin.ChainGetMessage',
        params: [{"/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"}],
        id: 'getblock.io'
    })
});

const data = await response.json();
console.log(data.result);
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
    json={
        'jsonrpc': '2.0',
        'method': 'Filecoin.ChainGetMessage',
        'params': [{"/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"}],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "Version": 0,
        "To": "f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq",
        "From": "f3ukhj6tpkgxjknu54opiaej2vrjz7nh7gzodkqlhpfphc6gxkogzmojv2cnlpgcuwkvnyhloctnc6lmlvceuq",
        "Nonce": 42,
        "Value": "1000000000000000000",
        "GasLimit": 2200000,
        "GasFeeCap": "101520",
        "GasPremium": "100178",
        "Method": 0,
        "Params": null
    }
}
```

## Response Parameters

| Field    | Type    | Description                                            |
| -------- | ------- | ------------------------------------------------------ |
| To       | string  | Recipient address                                      |
| From     | string  | Sender address                                         |
| Value    | string  | Amount transferred, in attoFIL (1 FIL = 10^18 attoFIL) |
| GasLimit | integer | Maximum gas units the message may consume              |
| Method   | integer | Actor method number invoked (0 for a plain send)       |

## Use Cases

* **Transaction Views**: Render a message's parties and value
* **Payment Confirmation**: Read a transfer's amount and recipient
* **Gas Analysis**: Inspect a message's gas parameters
* **Actor Calls**: Read which actor method a message invoked

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
