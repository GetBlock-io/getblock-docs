# eth\_getBalance

{% hint style="success" %}
The **eth\_getBalance** method returns the ETH balance of the specified address on Optimism at a given block.
{% endhint %}

This method retrieves the native ETH balance of any address on the Optimism network. The balance is returned in wei (the smallest unit of ETH, where 1 ETH = 10^18 wei). You can query the balance at a specific block height or use tags like 'latest', 'earliest', or 'pending'. This is essential for wallet applications, balance verification, and transaction validation.

## Supported Networks

The eth\_getBalance **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

## Parameters

{% hint style="info" %}
Two parameters are required:
{% endhint %}

| Parameter | Type           | Description                                              | Required |
| --------- | -------------- | -------------------------------------------------------- | -------- |
| address   | DATA, 20 Bytes | The address to check the balance of.                     | Yes      |
| block     | QUANTITY\|TAG  | Block number in hex, or 'latest', 'earliest', 'pending'. | Yes      |

## Request

### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getBalance **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getBalance", "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "latest"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

## Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1bc16d674ec80000"
}
```

## Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The balance in wei as a hexadecimal string. `0x1bc16d674ec80000` equals 2 ETH (2000000000000000000 wei).

## Use Case

The eth\_getBalance method is commonly used for:

* **Wallet balance display**
* **Transaction validation**
* **Account monitoring**
* **Portfolio tracking**

## Code Example

Here's how to use the eth\_getBalance method with different programming languages:

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
    "method": "eth_getBalance",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getBalance result:", result)
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
    method: "eth_getBalance",
    params: ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "latest"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getBalance result:", response.data.result);
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
