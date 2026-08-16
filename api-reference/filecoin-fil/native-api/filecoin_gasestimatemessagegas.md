---
description: >-
  Example code for the Filecoin.GasEstimateMessageGas JSON RPC method. Complete
  guide on how to use Filecoin.GasEstimateMessageGas JSON RPC in GetBlock Web3
  documentation.
---

# Filecoin.GasEstimateMessageGas - Filecoin

This method fills in the unset gas fields of a message, estimating the gas limit, fee cap, and premium needed for timely inclusion. The message is returned with its gas fields populated.

## Parameters

| Parameter | Type   | Required | Description                                                    |
| --------- | ------ | -------- | -------------------------------------------------------------- |
| message   | object | Yes      | The message to estimate gas for                                |
| sendSpec  | object | No       | Optional send specification, such as a max fee; null to omit   |
| tipsetKey | array  | Yes      | Tipset key to estimate against; empty array for the chain head |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.GasEstimateMessageGas", "params": [{"To": "f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq", "From": "f3ukhj6tpkgxjknu54opiaej2vrjz7nh7gzodkqlhpfphc6gxkogzmojv2cnlpgcuwkvnyhloctnc6lmlvceuq", "Value": "1000000000000000000", "Method": 0}, null, []], "id": "getblock.io"}'
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
        method: 'Filecoin.GasEstimateMessageGas',
        params: [{"To": "f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq", "From": "f3ukhj6tpkgxjknu54opiaej2vrjz7nh7gzodkqlhpfphc6gxkogzmojv2cnlpgcuwkvnyhloctnc6lmlvceuq", "Value": "1000000000000000000", "Method": 0}, null, []],
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
        'method': 'Filecoin.GasEstimateMessageGas',
        'params': [{"To": "f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq", "From": "f3ukhj6tpkgxjknu54opiaej2vrjz7nh7gzodkqlhpfphc6gxkogzmojv2cnlpgcuwkvnyhloctnc6lmlvceuq", "Value": "1000000000000000000", "Method": 0}, null, []],
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
        "To": "f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq",
        "From": "f3ukhj6tpkgxjknu54opiaej2vrjz7nh7gzodkqlhpfphc6gxkogzmojv2cnlpgcuwkvnyhloctnc6lmlvceuq",
        "Value": "1000000000000000000",
        "GasLimit": 2200000,
        "GasFeeCap": "101520",
        "GasPremium": "100178",
        "Method": 0,
        "Nonce": 43
    }
}
```

## Response Parameters

| Field      | Type    | Description                                     |
| ---------- | ------- | ----------------------------------------------- |
| GasLimit   | integer | Estimated maximum gas units for the message     |
| GasFeeCap  | string  | Estimated maximum fee per gas unit, in attoFIL  |
| GasPremium | string  | Estimated priority fee per gas unit, in attoFIL |
| Nonce      | integer | Nonce assigned to the message                   |

## Use Cases

* **Fee Estimation**: Populate gas fields before signing a message
* **Cost Preview**: Show the estimated cost of a message
* **Inclusion Tuning**: Set gas for timely block inclusion
* **Wallet Flows**: Prepare a fully specified message for signing

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
