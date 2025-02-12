---
description: >-
  Retrieve historical gas fee data using eth_feeHistory. Useful for transaction
  analysis, optimization, and planning by providing gas fees and usage ratios
  for a specified block range.
---

# eth\_feeHistory - Ethereum

{% hint style="success" %}
The eth\_feeHistory method is part of the Ethereum JSON-RPC API and allows you to retrieve historical gas information for Ethereum transactions
{% endhint %}

This method returns data on gas fees and usage ratios for a specified range of blocks, which can be used for transaction analysis, optimization, and other blockchain-related tasks. This method is part of the Core API for Ethereum and uses JSON RPC to communicate with the blockchain. It provides useful data for gas optimization and transaction planning.

### Supported Networks

The eth\_feeHistory RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesk

### **Parameters:**

* DATA, 8 Bytes — Number of blocks in the requested range.
* DATA, 32 Bytes — The latest block in the requested range.
* DATA, 32 Bytes — An optional monotonically increasing list of percentile values to sample from each block’s effective priority fees per gas in ascending order, weighted by gas used.

### Request

#### URL

{% code fullWidth="false" %}
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}

To make a request, you need to send a JSON object with the jsonrpc, method, and params fields. Here is an example of how to make a request using curl:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": [
        61,
        "latest",
        [20, 60, 73]
    ],
    "id": "getblock.io"
}'

```
{% endtab %}

{% tab title="wss" %}
```json
wscat -c wss://eth.getblock.io/YOUR-API-KEY/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_feeHistory",
"params": [61, "latest", [20, 60, 73]],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the requested gas fee history and related information. Here is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "baseFeePerGas": [
            "0x64191db63", "0x709685587", "0x6c169393c", "0x6d228f023",
            "0x6bc7f0cec", "0x6d912c34d", "0x711120783", "0x6f6916161"
        ],
        "gasUsedRatio": [
            0.9991048333333333, 0.3401327333333333, 0.5387389333333333, 
            0.38130176666666665, 0.5369746, 0.28408893333333335
        ],
        "oldestBlock": "0x1092353",
        "reward": [
            ["0x5f5e100", "0x5f5e100", "0x5f5e100"],
            ["0x5f5e100", "0x11e1a300", "0x59682f00"]
        ]
    }
}

```

### **Response Description:**

* baseFeePerGas: An array of base fees per gas for each block in the requested range.
* gasUsedRatio: An array representing the gas usage ratio for each block.
* oldestBlock: The number of the oldest block in the requested range.
* reward: An array containing reward information for each block for the specified percentiles.

### **Use Case:**

Let's say you're a decentralized application (DApp) developer who wants to optimize the gas fees for users interacting with your smart contract. By using the eth\_feeHistory method, you can retrieve historical gas data to predict the future gas prices and set a more efficient transaction fee for your users.

### **Code Example :**

You can also make requests to the eth\_feeHistory method programmatically using Python. Below is an example using the requests library:

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

# Define the API URL and access token
url = 'https://go.getblock.io/<ACCESS-TOKEN>/'
headers = {'Content-Type': 'application/json'}

# Prepare the request data
data = {
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": [
        61,  # Number of blocks
        "latest",  # The latest block
        [20, 60, 73]  # Percentiles
    ],
    "id": "getblock.io"
}

# Send the POST request
response = requests.post(url, headers=headers, data=json.dumps(data))

# Parse the JSON response
response_data = response.json()

# Print the result
print(json.dumps(response_data, indent=4))
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = { 'Content-Type': 'application/json' };

const data = {
    jsonrpc: "2.0",
    method: "eth_feeHistory",
    params: [
        61, 
        "latest", 
        [20, 60, 73] 
    ],
    id: "getblock.io"
};

axios.post(url, data, { headers })
    .then(response => {
        console.log(JSON.stringify(response.data, null, 4));
    })
    .catch(error => {
        console.error("Error:", error.response ? error.response.data : error.message);
    });

```
{% endtab %}
{% endtabs %}

This script sends a request to the eth\_feeHistory method and prints the returned historical gas information. Make sure to replace \<ACCESS-TOKEN> with your actual API token.

**Common Errors:**

eth\_feeHistory error: You might encounter errors if the provided parameters are incorrect, or if the network is unreachable. Also, ensure that the block range you are querying is valid, and that your access token is correctly provided.\\
