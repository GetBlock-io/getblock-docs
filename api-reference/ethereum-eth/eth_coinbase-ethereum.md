---
description: >-
  Retrieve the primary Ethereum client address using eth_coinbase. Useful for
  verifying the main configured account on the node for tasks like fee
  management or administrative purposes.
---

# eth\_coinbase - Ethereum

{% hint style="success" %}
The eth\_coinbase method returns the primary address (coinbase) of the connected Ethereum client.
{% endhint %}

In the context of modern Ethereum nodes, this method can be used to retrieve the configured primary address, which is often set in the client configuration for administrative or service tasks. While this address was previously used to receive mining rewards in the Proof-of-Work (PoW) era, in the current Proof-of-Stake (PoS) network, it can be useful for verifying the main configured account on the node, such as for obtaining information about the wallet in use or the address designated for fees or other purposes.

The eth\_coinbase method is often used to verify the served eth\_coinbase address, ensuring that the correct Ethereum address is set for the node’s tasks, such as fee management.

### Supported Networks

The **`eth_chainId` RPC Ethereum** method supports the following network types

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

The following example demonstrates how to make a JSON RPC request to fetch the coinbase address using the eth\_coinbase method. This is especially useful in Ethereum setups with active mining to verify the designated mining address.

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw {
    "jsonrpc": "2.0",
    "method": "eth_coinbase",
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
"method": "eth_coinbase",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

In this request:

method: Specifies eth\_coinbase, the RPC method used to fetch the client’s mining address.

params: An empty array, as no parameters are needed for this method.

id: Identifies the request source, here set as "getblock.io".

### Response

If the node is not configured with a coinbase address, an error response similar to the following may be returned

```json
{
    "error": {
        "code": -32000,
        "message": "etherbase must be explicitly specified"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```

Explanation:\
The error message "etherbase must be explicitly specified" indicates that the coinbase address is not set on the Ethereum client. To resolve this, configure the client with the --miner-coinbase option when starting the node, specifying a valid Ethereum address (e.g., an address from MetaMask or Etherscan).

In cases where the eth\_coinbase method is correctly configured, the response will include the Ethereum coinbase address in hexadecimal format:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0xYourCoinbaseAddressHere"
}

```

### Use Case

The eth\_coinbase method in the Core API can now be used to verify the reward address settings for a staker managing a node (validator). This is particularly useful for checking the node configuration to ensure that rewards for block validation are correctly directed to the designated address. The method is typically used alongside eth\_chainId and eth\_syncing for verifying the current network and synchronization status, helping to confirm that the node is properly configured within the Ethereum network.

### Code Example

Below is a Python script that demonstrates how to call the eth\_coinbase method using the JSON-RPC protocol

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

# Define the API endpoint URL and headers
url = "https://go.getblock.io/<ACCESS-TOKEN>/"  # Replace <ACCESS-TOKEN> with your actual API token
headers = {
    "Content-Type": "application/json"
}

# Create the payload for the eth_coinbase method request
payload = {
    "jsonrpc": "2.0",
    "method": "eth_coinbase",
    "params": [],  # No parameters required for eth_coinbase
    "id": "getblock.io"
}

# Send the POST request to the Ethereum node
response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check if the response status code is 200 (OK)
if response.status_code == 200:
    # Extract the 'result' field or handle the error message
    result = response.json().get("result")
    if result:
        print(f"Coinbase Address: {result}")
    else:
        error_message = response.json().get("error", {}).get("message")
        print(f"Error: {error_message}")
else:
    # Print an error message if the request fails
    print("Error:", response.status_code, response.text)

```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

// Define the API endpoint URL and headers
const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; // Replace <ACCESS-TOKEN> with your actual API token
const headers = { "Content-Type": "application/json" };

// Create the payload for the eth_coinbase method request
const payload = {
    jsonrpc: "2.0",
    method: "eth_coinbase",
    params: [], // No parameters required for eth_coinbase
    id: "getblock.io"
};

// Send the POST request to the Ethereum node
axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            // Extract the 'result' field or handle the error message
            const result = response.data.result;
            if (result) {
                console.log(`Coinbase Address: ${result}`);
            } else {
                const errorMessage = response.data.error?.message || "Unknown error";
                console.log(`Error: ${errorMessage}`);
            }
        } else {
            // Handle unexpected HTTP statuses
            console.log(`Error: ${response.status} - ${response.statusText}`);
        }
    })
    .catch(error => {
        // Handle network or server errors
        console.error("Error:", error.response ? error.response.data : error.message);
    });

```
{% endtab %}
{% endtabs %}

API Call Setup: The script sends a JSON-RPC request using the eth\_coinbase method to retrieve the mining address.Error Handling: It checks for a successful response (status code 200) and handles potential errors, such as the coinbase address not being specified.Output: The coinbase address is printed if available. If an error occurs (e.g., "etherbase must be explicitly specified"), the error message is displayed instead.
