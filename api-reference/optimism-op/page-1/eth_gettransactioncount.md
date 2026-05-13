# eth\_getTransactionCount

{% hint style="success" %}
The **eth\_getTransactionCount** method returns the nonce (transaction count) for an address on Optimism.
{% endhint %}

This method returns the number of transactions sent from a specific address, which is also known as the nonce. The nonce is critical for transaction ordering and preventing replay attacks. When sending transactions, you need to use the correct nonce value. Using `'pending'` as the block parameter returns the nonce including pending transactions, which is essential for sending multiple transactions in sequence.

### Supported Networks

The eth\_getTransactionCount **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
Two parameters are required:
{% endhint %}

| Parameter | Type           | Description                                                    | Required |
| --------- | -------------- | -------------------------------------------------------------- | -------- |
| address   | DATA, 20 Bytes | The address to get the transaction count for.                  | Yes      |
| block     | QUANTITY\|TAG  | Block number in hex, or `'latest'`, `'earliest'`, `'pending'`. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getTransactionCount **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getTransactionCount", "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "latest"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x29"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The transaction count (nonce) as a hexadecimal string. `0x29` equals 41 transactions sent from this address.

### Use Case

The eth\_getTransactionCount method is commonly used for:

* **Transaction nonce management**
* **Account activity analysis**
* **Transaction sequencing**
* **Wallet implementation**

### Code Example

Here's how to use the eth\_getTransactionCount method with different programming languages:

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
    "method": "eth_getTransactionCount",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getTransactionCount result:", result)
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
    method: "eth_getTransactionCount",
    params: ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "latest"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getTransactionCount result:", response.data.result);
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
