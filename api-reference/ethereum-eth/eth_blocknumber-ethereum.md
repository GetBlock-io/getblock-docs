---
description: >-
  Retrieve the latest Ethereum block number using eth_blockNumber. Ideal for
  tracking blockchain progress and keeping dApps synchronized with the current
  chain height via JSON-RPC.
---

# eth\_blockNumber - Ethereum

{% hint style="success" %}
The **eth\_blockNumber** method in Ethereum retrieves the latest block number from the current chain head, offering real-time insight into the blockchain's progress.
{% endhint %}

This method is essential for applications that need to stay updated with the latest state of the blockchain without diving into detailed block data. By using this RPC method, Web3 developers can maintain synchronized blockchain data with minimal overhead, and it provides a straightforward way to gauge the chain’s current height. This method is available through specific **API endpoints** that are part of the **Core API**, making it easy for developers to access blockchain data. The **eth\_blockNumber** method is particularly useful for monitoring the state of the **transaction** chain and ensuring that applications remain synchronized with the latest block height.

### Supported Networks

The eth\_blockNumber **RPC Ethereum** method supports the following network types

* **Mainnet**
* **Testnet**: Sepolia, Holesky

### Parameters

{% hint style="info" %}
This method does not accept any parameters
{% endhint %}

### Request

#### URL

{% code fullWidth="false" %}
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}

To request the chain ID using the eth\_blockNumber **RPC Ethereum** method via JSON-RPC, use the following **curl** command:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw {
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
}
```
{% endtab %}

{% tab title="wss" %}
```json
wscat -c wss://eth.getblock.io/YOUR-API-KEY/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_blockNumber",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response provides the latest block number as a hexadecimal value

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x109238e"
}
```

### Body Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The method result:
  * A string representing the latest block number in the Ethereum blockchain, provided in HEX format.
  * For example, `"0x109238e"` in decimal equals `10923886`.

### Use Case

The eth\_blockNumber method is ideal for applications needing to check the latest Ethereum block number quickly. For instance, Web3 applications can use it to verify the synchronization state with the Ethereum network, monitor blockchain progression, or trigger actions based on block height conditions. By contrast, other methods like eth\_call are used for data retrieval within contracts rather than simply identifying the current block number.

### Code Example

Here’s a Python example using the eth\_blockNumber method to get the current chain head in Web3

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/c8de489a140b4b80a0ed13f264cc4f65/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    # Convert hexadecimal to decimal
    block_number = int(response.json().get("result"), 16)
    print("Current Block Number:", block_number)
else:
    print("Error:", response.status_code, response.text)

```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

// Define the API URL and headers
const url = "https://go.getblock.io/c8de489a140b4b80a0ed13f264cc4f65/";
const headers = { "Content-Type": "application/json" };

// Prepare the request payload
const payload = {
    jsonrpc: "2.0",
    method: "eth_blockNumber",
    params: [],
    id: "getblock.io"
};

// Send the POST request
axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            // Extract the block number from the response
            const hexBlockNumber = response.data.result;

            // Convert the hexadecimal block number to decimal
            const blockNumber = parseInt(hexBlockNumber, 16);

            // Print the current block number
            console.log("Current Block Number:", blockNumber);
        } else {
            // Handle unexpected HTTP statuses
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        // Handle network or server errors
        console.error("Error:", error.response ? error.response.data : error.message);
    });

```
{% endtab %}
{% endtabs %}

This Python example utilizes **eth\_blockNumber** to retrieve the latest block index in decimal format, making it easy to incorporate into applications that need to monitor Ethereum’s current block. With this method, developers can programmatically stay updated on chain status, enabling features like block synchronization and blockchain-based triggers in Web3 projects. If an **eth\_blockNumber error** occurs, it could indicate issues with the connection to the blockchain, improper request formatting, or network-related problems, and should be handled appropriately to ensure smooth operation.
