# rollup\_gasPrices

{% hint style="success" %}
The **rollup\_gasPrices** method returns both L1 and L2 gas prices for Optimism transactions.
{% endhint %}

This Optimism-specific method returns the current gas prices for both the L1 (Ethereum) and L2 (Optimism) components of transaction costs. On Optimism, transactions have two cost components: the L2 execution cost and the L1 data availability cost. This method helps applications accurately estimate total transaction costs. Note: The official Optimism documentation recommends using `eth_gasPrice` for L2 prices and the GasPriceOracle contract for L1 fees.

### Supported Networks

The rollup\_gasPrices **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
This method does not accept any parameters.
{% endhint %}

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the rollup\_gasPrices **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "rollup_gasPrices",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "rollup_gasPrices", "params": [], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "l1GasPrice": "0x2540be400",
        "l2GasPrice": "0x3b9aca00"
    }
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: An object containing l1GasPrice (L1 data cost component) and l2GasPrice (L2 execution cost) in wei as hexadecimal strings.

### Use Case

The rollup\_gasPrices method is commonly used for:

* **Accurate fee estimation**
* **L1 data cost calculation**
* **Transaction cost optimization**
* **DApp fee display**

### Code Example

Here's how to use the rollup\_gasPrices method with different programming languages:

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
    "method": "rollup_gasPrices",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("rollup_gasPrices result:", result)
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
    method: "rollup_gasPrices",
    params: [],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("rollup_gasPrices result:", response.data.result);
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
