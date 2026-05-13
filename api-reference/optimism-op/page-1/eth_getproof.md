# eth\_getProof

{% hint style="success" %}
The **eth\_getProof** method returns the account and storage values with Merkle proofs for verification on Optimism.
{% endhint %}

This method returns the account information (balance, nonce, codeHash, storageHash) along with Merkle proofs that can be used to verify the data against the state root. It also returns proofs for specified storage keys. This is essential for light clients and cross-chain verification, enabling trustless verification of account and storage state.

### Supported Networks

The eth\_getProof **RPC Optimism** method supports the following network types:

* **Mainnet**
* **Testnet**: Sepolia

### Parameters

{% hint style="info" %}
Three parameters are required:
{% endhint %}

| Parameter   | Type           | Description                                              | Required |
| ----------- | -------------- | -------------------------------------------------------- | -------- |
| address     | DATA, 20 Bytes | The address to get proof for.                            | Yes      |
| storageKeys | Array          | Array of storage keys to get proofs for.                 | Yes      |
| block       | QUANTITY\|TAG  | Block number in hex, or 'latest', 'earliest', 'pending'. | Yes      |

### Request

#### URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request using the eth\_getProof **RPC Optimism** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", ["0x0"], "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "eth_getProof", "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", ["0x0"], "latest"], "id": "getblock.io"}
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
        "address": "0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e",
        "accountProof": ["0xf90211a0..."],
        "balance": "0x1bc16d674ec80000",
        "codeHash": "0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
        "nonce": "0x1",
        "storageHash": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "storageProof": []
    }
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: An object containing account proof, balance, codeHash, nonce, storageHash, and storage proofs for verification.

### Use Case

The eth\_getProof method is commonly used for:

* **Light client verification**
* **Cross-chain proofs**
* **State verification**
* **Trustless data access**

### Code Example

Here's how to use the eth\_getProof method with different programming languages:

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
    "method": "eth_getProof",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", ["0x0"], "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getProof result:", result)
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
    method: "eth_getProof",
    params: ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", ["0x0"], "latest"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getProof result:", response.data.result);
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
