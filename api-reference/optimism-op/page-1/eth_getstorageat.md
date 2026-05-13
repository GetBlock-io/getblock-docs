# eth\_getStorageAt

{% hint style="success" %}
The **eth\_getStorageAt** method returns the raw value stored at a specific storage slot of a contract on Optimism.
{% endhint %}

This method reads directly from a contract's storage slots. It's a low-level method that requires knowledge of the contract's storage layout. Each storage slot holds 32 bytes of data. This is useful for reading contract state that isn't exposed through public functions or for debugging and analysis purposes.

### Supported Networks

The eth\_getStorageAt **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
Three parameters are required:
{% endhint %}

| Parameter | Type           | Description                                              | Required |
| --------- | -------------- | -------------------------------------------------------- | -------- |
| address   | DATA, 20 Bytes | The address of the contract.                             | Yes      |
| position  | QUANTITY       | The storage position (slot) as a hexadecimal.            | Yes      |
| block     | QUANTITY\|TAG  | Block number in hex, or 'latest', 'earliest', 'pending'. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getStorageAt **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getStorageAt", "params": ["0x4200000000000000000000000000000000000006", "0x0", "latest"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x0000000000000000000000000000000000000000000000000000000000000001"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The 32-byte value at the storage position as a hexadecimal string.

### Use Case

The eth\_getStorageAt method is commonly used for:

* **Contract state analysis**
* **Debugging**
* **Storage layout exploration**
* **Security auditing**

### Code Example

Here's how to use the eth\_getStorageAt method with different programming languages:

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
    "method": "eth_getStorageAt",
    "params": ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getStorageAt result:", result)
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
    method: "eth_getStorageAt",
    params: ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getStorageAt result:", response.data.result);
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
