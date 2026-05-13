# web3\_sha3

{% hint style="success" %}
The **web3\_sha3** method returns the Keccak-256 hash of the provided data.
{% endhint %}

This method computes and returns the Keccak-256 hash (NOT standard SHA3-256) of the input data. Keccak-256 is the hash function used throughout Ethereum and Optimism for computing addresses, transaction hashes, and other cryptographic operations. The input must be a hexadecimal string.

### Supported Networks

The web3\_sha3 **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter | Type | Description                                | Required |
| --------- | ---- | ------------------------------------------ | -------- |
| data      | DATA | The data to hash, as a hexadecimal string. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the web3\_sha3 **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "web3_sha3",
    "params": ["0x68656c6c6f"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "web3_sha3", "params": ["0x68656c6c6f"], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1c8aff950685c2ed4bc3174f3472287b56d9517b9c948127319a09a7a36deac8"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The Keccak-256 hash of the input data as a 32-byte hexadecimal string.

### Use Case

The web3\_sha3 method is commonly used for:

* **Data hashing**
* **Signature verification**
* **Address computation**
* **Cryptographic operations**

### Code Example

Here's how to use the web3\_sha3 method with different programming languages:

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
    "method": "web3_sha3",
    "params": ["0x68656c6c6f"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("web3_sha3 result:", result)
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
    method: "web3_sha3",
    params: ["0x68656c6c6f"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("web3_sha3 result:", response.data.result);
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
