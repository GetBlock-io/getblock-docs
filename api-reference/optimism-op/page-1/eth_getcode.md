# eth\_getCode

{% hint style="success" %}
The **eth\_getCode** method returns the compiled bytecode of a smart contract deployed at a given address on Optimism.
{% endhint %}

This method retrieves the EVM bytecode stored at a specific address. If the address is an externally owned account (EOA) or an empty address, it returns `'0x'`. This is useful for verifying contract deployment, analyzing contracts, and checking if an address is a contract or EOA. The bytecode is the compiled version of the smart contract's source code.

### Supported Networks

The eth\_getCode **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
Two parameters are required:
{% endhint %}

| Parameter | Type           | Description                                                    | Required |
| --------- | -------------- | -------------------------------------------------------------- | -------- |
| address   | DATA, 20 Bytes | The address to retrieve bytecode from.                         | Yes      |
| block     | QUANTITY\|TAG  | Block number in hex, or `'latest'`, `'earliest'`, `'pending'`. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getCode **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0x4200000000000000000000000000000000000006", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getCode", "params": ["0x4200000000000000000000000000000000000006", "latest"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x608060405234801561001057600080fd5b50..."
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The bytecode at the address as a hexadecimal string. Returns `'0x'` for EOAs or empty addresses.

### Use Case

The eth\_getCode method is commonly used for:

* **Contract verification**
* **EOA vs contract detection**
* **Security analysis**
* **Bytecode comparison**

### Code Example

Here's how to use the eth\_getCode method with different programming languages:

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
    "method": "eth_getCode",
    "params": ["0x4200000000000000000000000000000000000006", "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getCode result:", result)
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
    method: "eth_getCode",
    params: ["0x4200000000000000000000000000000000000006", "latest"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getCode result:", response.data.result);
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
