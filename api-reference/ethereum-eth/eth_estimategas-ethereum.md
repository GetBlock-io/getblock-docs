---
description: >-
  Estimate the gas required for a transaction using eth_estimateGas. Crucial for
  predicting gas costs and preventing out-of-gas errors in Web3 applications.
---

# eth\_estimateGas - Ethereum

{% hint style="success" %}
The eth\_estimateGas method is an essential part of the JSON RPC Ethereum protocol. It provides an estimate of the gas required for a transaction to execute successfully on the Ethereum network.
{% endhint %}

This estimation allows developers to predict gas costs before submitting a transaction, preventing potential out-of-gas errors. Importantly, this process does not consume gas, and the transaction is not added to the blockchain during estimation.

Using the eth\_estimateGas method is a crucial practice for optimizing transaction costs in Web3 applications and ensuring that enough gas is provided to avoid failures. This is particularly important when interacting with Ethereum's eth\_estimateGas API and its Endpoints.

### Supported Networks

The eth\_estimateGas **RPC Ethereum** method supports the following network types

* **Mainnet**
* **Testnet**: Sepolia, Hoodi

### Parameters

The eth\_estimateGas method requires a Transaction Call Object to estimate the necessary gas. The key fields of the transaction call object include:

* from (optional): The address the transaction is sent from.
* to: The destination address where the transaction is directed.
* value (optional): The amount of Ether to be sent, represented in hexadecimal.

If the Ethereum node is started with --revert-reason-enabled, the eth\_estimateGas error response may include the reason for the transaction reversion (execution reverted).

### Request

#### URL

{% code fullWidth="false" %}
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}

Below is an example of a JSON-RPC request using the eth\_estimateGas method to estimate the gas required for a specific transaction.

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0xFE3B557E8Fb62b89F4916B721be55cEb828dBd73",
            "to": "0x44Aa93095D6749A706051658B970b941c72c1D53",
            "value": "0x1"
        }
    ],
    "id": "getblock.io"
}'

```
{% endtab %}

{% tab title="wss" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_estimateGas",
"params": [{"from": "0xFE3B557E8Fb62b89F4916B721be55cEb828dBd73", "to": "0x44Aa93095D6749A706051658B970b941c72c1D53", "value": "0x1"}],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

In this request:

* method: Specifies eth\_estimateGas, the RPC method used to estimate gas.
* params: Contains the transaction call object with from, to, and value fields.
* id: Identifies the request, set as "getblock.io".

If the Ethereum node is started with --revert-reason-enabled, the eth\_estimateGas error response may include the reason for transaction reversion (execution reverted).

### Response

The response contains the estimated gas amount in hexadecimal format

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x5208"
}
```

The result field ("0x5208") represents the estimated gas required for the transaction, which is 21000 in decimal. This is the standard gas amount for a simple Ether transfer.

### Error Handling

If the Ethereum node is started with --revert-reason-enabled, the eth\_estimateGas error response may include the reason for the transaction reversion (execution reverted).

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "error": {
        "code": -32000,
        "message": "execution reverted: Reason for failure"
  }
}
```

This error response indicates that the transaction execution was reverted due to a problem in the transaction call, which is often seen when interacting with smart contracts.

### Use Case

The eth\_estimateGas RPC method is widely used in Ethereum Web3 Core API applications to calculate the gas cost for transactions before they are sent. It helps ensure that the sender has enough Ether to cover the gas fees, preventing transaction failures due to insufficient gas.

For example, if a user interacts with a decentralized application (dApp) using MetaMask and attempts a transaction, the eth\_estimateGas method can be used to verify that the necessary gas is available before sending the transaction. If the dApp detects that the transaction will fail due to insufficient gas, it can alert the user to add more gas to the transaction.

### Code Example

Below is a Python script to demonstrate how to use the eth\_estimateGas method with JSON-RPC protocol.

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

# API endpoint and headers
url = "https://go.getblock.io/<ACCESS-TOKEN>/"  # Replace <ACCESS-TOKEN> with your API key
headers = {
    "Content-Type": "application/json"
}

# Define the transaction call object
transaction = {
    "from": "0xFE3B557E8Fb62b89F4916B721be55cEb828dBd73",
    "to": "0x44Aa93095D6749A706051658B970b941c72c1D53",
    "value": "0x1"
}

# Create the payload for the eth_estimateGas request
payload = {
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [transaction],
    "id": "getblock.io"
}

# Send the POST request
response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check if the request was successful
if response.status_code == 200:
    result = response.json().get("result")
    if result:
        # Print the estimated gas value
        print(f"Estimated Gas: {int(result, 16)}")
    else:
        # Print error message if any
        error_message = response.json().get("error", {}).get("message")
        print(f"Error: {error_message}")
else:
    print("Request failed with status:", response.status_code)

```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

// API endpoint and headers
const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; // Replace <ACCESS-TOKEN> with your API key
const headers = { "Content-Type": "application/json" };

// Define the transaction call object
const transaction = {
    from: "0xFE3B557E8Fb62b89F4916B721be55cEb828dBd73",
    to: "0x44Aa93095D6749A706051658B970b941c72c1D53",
    value: "0x1"
};

// Create the payload for the eth_estimateGas request
const payload = {
    jsonrpc: "2.0",
    method: "eth_estimateGas",
    params: [transaction],
    id: "getblock.io"
};

// Send the POST request
axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            const result = response.data.result;
            if (result) {
                // Convert the hexadecimal gas value to a decimal and log it
                console.log(`Estimated Gas: ${parseInt(result, 16)}`);
            } else {
                // Log the error message from the response
                const errorMessage = response.data.error?.message || "Unknown error occurred";
                console.log(`Error: ${errorMessage}`);
            }
        } else {
            console.log(`Request failed with status: ${response.status}`);
        }
    })
    .catch(error => {
        // Handle network or server errors
        console.error("Error:", error.response ? error.response.data : error.message);
    });

```
{% endtab %}
{% endtabs %}

