---
description: >-
  Access mining status on BSC with eth_mining via the JSON-RPC API Interface. Quickly check if mining is active and running smoothly.
---

# eth_mining

{% hint style="success" %}
The RPC method checks if a BSC node is actively mining, primarily used to verify mining activity status.&#x20;
{% endhint %}

The `eth_mining` method in the JSON-RPC protocol is used to check if the client is actively mining new blocks in the BSC network. By invoking `eth_mining` through Web3, developers can programmatically determine the mining status, which is crucial for applications that depend on real-time block generation.

Utilizing the `eth_mining` RPC protocol, this method returns a Boolean value: `true` if mining is ongoing, and `false` otherwise. This functionality is essential for monitoring and managing mining operations, ensuring that systems relying on block confirmations can adapt to the current state of the network.

## Supported Networks

The eth_mining JSON-RPC API method supports the following network types:
- **Mainnet**
- **Testnet**

## Parameters

None: This method does not require any parameters.

# Request Example

##### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```


#### Request

Here’s a sample cURL request using eth_mining :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_mining",
  "params": [],
  "id": 67
}
```
{% endtab %}
{% endtabs %}

#### Response

Below is a sample JSON response returned by eth_mining upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": 67,
  "result": false
}

```

## Body Parameters

Here is the list of body parameters for `eth_mining` method:

1. **`jsonrpc`**: A string specifying the version of the JSON-RPC protocol. In this case, it is "2.0".

2. **`id`**: A unique identifier for the request. This is typically a number, string, or null. In the example, it is 67.

3. **`result`**: A boolean value that indicates whether the client is actively mining new blocks. In this example, the value is `false`.

## Use Cases

Here are some use-cases for `eth_mining` method:

1. **Monitoring Mining Status**: The `eth_mining` method can be used to check if the Ethereum client is currently mining. This is particularly useful for developers and administrators who need to monitor the status of mining operations in real time. By periodically calling this method, they can ensure that mining is active and troubleshoot any issues if mining unexpectedly stops.

2. **Dynamic Application Behavior**: In decentralized applications (dApps) that are sensitive to the mining status, the `eth_mining` method can be used to dynamically adjust the application's behavior. For instance, a dApp might provide different user interfaces or functionalities depending on whether mining is active, allowing for more efficient resource allocation and user interaction.

3. **Automated Alerts and Notifications**: Developers can integrate the `eth_mining` method into scripts or services that automatically send alerts or notifications when the mining status changes. This can be useful for mining pool operators or individual miners who want to receive immediate updates if their mining operations are interrupted, enabling them to take timely corrective actions.

## Code for eth_mining

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
  "id": 67,
  "result": false
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)

```
{% endtab %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  "jsonrpc": "2.0",
  "id": 67,
  "result": false
};

axios.post(url, payload, {
  headers: { "Content-Type": "application/json" }
})
.then(response => {
  console.log("Result:", response.data.result);
})
.catch(error => {
  if (error.response) {
    console.error("Error:", error.response.status, error.response.data);
  } else {
    console.error("Request failed:", error.message);
  }
});
```
{% endtab %}
{% endtabs %}

## Common Errors

When using the `eth_mining` JSON-RPC API BSC method, the following issues may occur:
- The method returns `false` even when mining is expected to be active. This could be due to the node not being properly configured for mining. Ensure that the node's configuration file has mining enabled and that it is connected to a network with sufficient peers.
- A timeout error occurs when querying the mining status. This may happen if the node is under heavy load or if there's a network connectivity issue. Check the node's resource usage and network connection to resolve this.
- The method returns inconsistent results across different nodes. This can occur if nodes are not synchronized with the latest block data. Verify that all nodes are fully synced with the blockchain to ensure consistent mining status.

Using the `eth_mining` method in Web3 applications allows developers to programmatically check if a node is actively mining, which is crucial for applications that depend on mining operations. By integrating this method, developers can create more responsive and reliable applications that can adapt to the mining state of the network in real-time.

## Conclusion

The JSON-RPC method `eth_mining` is used to check if the Ethereum client is actively mining new blocks. While this method is pertinent to Ethereum, it is not applicable to the Binance Smart Chain (BSC), as BSC uses a different consensus mechanism. Understanding `eth_mining` is crucial for developers and miners who rely on JSON-RPC for blockchain interactions.
