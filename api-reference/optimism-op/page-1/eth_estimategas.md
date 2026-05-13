# eth\_estimateGas

{% hint style="success" %}
The **eth\_estimateGas** method estimates the amount of L2 gas required to execute a transaction on Optimism.
{% endhint %}

This method simulates a transaction and returns an estimate of the L2 gas that would be consumed. The estimate may be higher than the actual gas used because it accounts for the worst-case execution path. Important: On Optimism, this only estimates L2 execution gas. The total transaction cost also includes L1 data fees. For complete cost estimation, use the GasPriceOracle contract or Optimism SDK.

### Supported Networks

The eth\_estimateGas **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
One or two parameters:
{% endhint %}

| Parameter   | Type          | Description                                                                                                                                     | Required |
| ----------- | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| transaction | Object        | Transaction call object with from (optional), to (optional), gas (optional), gasPrice (optional), value (optional), and data (optional) fields. | Yes      |
| block       | QUANTITY\|TAG | (Optional) Block number in hex, or 'latest', 'earliest', 'pending'.                                                                             | No       |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_estimateGas **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "to": "0x8ba1f109551bD432803012645Ac136dff489ca2c", "value": "0xde0b6b3a7640000"}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_estimateGas", "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "to": "0x8ba1f109551bD432803012645Ac136dff489ca2c", "value": "0xde0b6b3a7640000"}], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x5208"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The estimated L2 gas amount as a hexadecimal string. `0x5208` equals 21,000 gas (standard ETH transfer).

### Use Case

The eth\_estimateGas method is commonly used for:

* **Transaction cost estimation**
* **Gas limit setting**
* **Pre-transaction validation**
* **DApp UX optimization**

### Code Example

Here's how to use the eth\_estimateGas method with different programming languages:

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "to": "0x8ba1f109551bD432803012645Ac136dff489ca2c", "value": "0xde0b6b3a7640000"}],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_estimateGas result:", result)
else:
    print("Error:", response.status_code, response.text)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    method: "eth_estimateGas",
    params: [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "to": "0x8ba1f109551bD432803012645Ac136dff489ca2c", "value": "0xde0b6b3a7640000"}],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_estimateGas result:", response.data.result);
        } else {
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        console.error("Error:", error.response ? error.response.data : error.message);
    });
```
{% endtab %}
{% endtabs %}

If an error occurs, it could indicate connection issues, invalid parameters, or network problems. Always implement proper error handling in your applications.
