---
description: >-
  Access the JSON-RPC API Interface to track event updates using
  eth_getFilterChanges in the BSC protocol, ensuring efficient data retrieval.
---

# eth\_getFilterChanges - Binance Smart Chain

{% hint style="success" %}
The method retrieves changes in logs or events for a specified filter on Binance Smart Chain, enabling efficient event tracking without polling the entire blockchain.
{% endhint %}

The `eth_getFilterChanges` method in the BSC protocol retrieves a list of changes since the last call to this method using the filter ID. In the context of `eth_getFilterChanges Web3`, it efficiently monitors blockchain events. This method is crucial for applications needing real-time updates without polling the entire blockchain.

When using the `eth_getFilterChanges RPC protocol`, it returns logs or transaction hashes, depending on the filter type. This enables developers to manage data streams effectively and maintain optimal performance. It's essential for developers implementing event-driven architectures on the Binance Smart Chain.

### Supported Networks

The eth\_getFilterChanges JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_getFilterChanges` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Filter ID**
  * **Type**: String
  * **Description**: The ID of the filter for which to retrieve changes. This ID is obtained from a previous filter creation call such as `eth_newFilter` or `eth_newBlockFilter`.
  * **Required**: Yes
  * **Default/Supported Values**: Must be a valid filter ID string returned by a filter creation method.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using eth\_getFilterChanges :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getFilterChanges",
  "params": ["YOUR_FILTER_ID"],
  "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by eth\_getFilterChanges upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "error": {
    "code": -32000,
    "message": "filter not found"
  }
}

```

### Body Parameters

Here is the list of body parameters for `eth_getFilterChanges` method:

1. **jsonrpc**: The version of the JSON-RPC protocol. It should be "2.0".
2. **id**: An identifier for the request, which can be any string or number. It is used to match the response with the request.
3. **error**: An object containing error information if the request was unsuccessful. This includes:
   * **code**: A numeric code indicating the type of error. In this case, the code is -32000.
   * **message**: A descriptive message providing more details about the error. Here, the message is "filter not found".

### Use Cases

Here are some use-cases for `eth_getFilterChanges` method:

1. **Monitoring Contract Events**: One of the primary use-cases for the `eth_getFilterChanges` method is to monitor events emitted by smart contracts. Developers can create a filter to listen for specific events and use this method to retrieve any new occurrences of these events since the last poll. This is particularly useful for applications that need to react to blockchain events in near real-time, such as updating UI elements in decentralized applications (dApps) when a specific event occurs.
2. **Tracking Transactions**: Another use-case is tracking transactions to or from specific addresses. By setting up a filter for transactions involving certain addresses, developers can use `eth_getFilterChanges` to get updates on new transactions. This can be useful for wallets or exchanges that need to display the latest transaction history to their users without polling the entire blockchain repeatedly.
3. **Network Monitoring and Analytics**: Developers and analysts can use `eth_getFilterChanges` to gather data for network monitoring and analytics. By setting up various filters, they can collect data on network activity, such as the number of transactions per block or the frequency of specific events, and analyze this information to gain insights into network performance and usage patterns.

### Code for eth\_getFilterChanges

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
  "id": "getblock.io",
  "error": {
    "code": -32000,
    "message": "filter not found"
  }
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
  "id": "getblock.io",
  "error": {
    "code": -32000,
    "message": "filter not found"
  }
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

### Common Errors

When using the `eth_getFilterChanges` JSON-RPC API BSC method, the following issues may occur:

* **Invalid Filter ID:** If the filter ID is incorrect or expired, the method will return an error. Ensure that the filter ID is valid and has not been removed by the network due to inactivity.
* **Network Latency:** High network latency can cause delayed responses or timeouts. To mitigate this, consider implementing retry logic or increasing the timeout settings in your application.
* **Empty Results:** The method may return an empty array if no events have occurred since the last poll. Verify that the filter is set up correctly and that there are indeed events to capture.
* **Permission Denied:** If the node does not permit access to create or query filters, you may encounter permission errors. Check the node's configuration and ensure your client has the necessary permissions.

Using the `eth_getFilterChanges` method in Web3 applications provides an efficient way to poll for changes in blockchain events without constantly querying the blockchain. This method allows developers to build responsive applications that react to on-chain events in real-time, improving the overall user experience and reducing unnecessary network load.

### Conclusion

The `eth_getFilterChanges` method in JSON-RPC is a powerful tool for developers working on Ethereum and Binance Smart Chain (BSC) networks. It allows them to efficiently track changes and updates to specific filters, enabling real-time data retrieval and event monitoring. This functionality is crucial for applications that require up-to-date blockchain data without constantly polling the network.
