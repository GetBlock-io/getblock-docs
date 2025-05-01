---
description: >-
  eth_newBlockFilter in JSON-RPC API Interface creates a filter to notify when
  new blocks are added to the BSC blockchain, enhancing monitoring.
---

# eth\_newBlockFilter - BNB Smart Chain

{% hint style="success" %}
The RPC method creates a filter to receive notifications for new blocks on the BNB Smart Chain, enabling real-time block monitoring.
{% endhint %}

The `eth_newBlockFilter` method in the BSC protocol is part of the `eth_newBlockFilter` Web3 and `eth_newBlockFilter` RPC protocol, designed to create a filter for new block notifications. This method enables users to listen for new blocks added to the blockchain, providing a unique filter identifier upon success.

Utilizing `eth_newBlockFilter` allows developers to efficiently track blockchain changes without polling for new blocks continuously. By leveraging this method, applications can respond to new block events in real-time, enhancing the responsiveness and efficiency of blockchain-based applications.

### Supported Networks

The `eth_newBlockFilter` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

None: This method does not require any parameters.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_newBlockFilter` :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_newBlockFilter",
  "params": [],
  "id": 67
}
```
{% endtab %}

{% tab title="wss" %}
```json
wscat -c wss://go.getblock.io/{ACCESS_TOKEN}/
{
    "jsonrpc": "2.0",
    "method": "eth_newBlockFilter",
    "params": [],
    "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by `eth_newBlockFilter` upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": 67,
  "result": "0xcb5b0ec347fb06c786ab6f6f3b4bb584"
}

```

### Body Parameters

Here is the list of body parameters for the `eth_newBlockFilter` method:

1. **jsonrpc**: The version of the JSON-RPC protocol used, which is "2.0" in this case.
2. **id**: A unique identifier for the request, which helps in matching the response with the request. In this example, it is 67.
3. **result**: The filter identifier returned by the method, which can be used to check for new blocks. In this example, it is "0xcb5b0ec347fb06c786ab6f6f3b4bb584".

### Use Cases

Here are some use-cases for `eth_newBlockFilter` method:

1. **Real-time Block Monitoring**: The `eth_newBlockFilter` method is particularly useful for applications that need to monitor new blocks in real-time. By creating a filter, developers can receive notifications whenever a new block is mined. This is essential for applications such as blockchain explorers, which need to update their data continuously to provide users with the latest information on block transactions, confirmations, and other block-related data.
2. **Smart Contract Event Tracking**: Developers can use `eth_newBlockFilter` to track events emitted by smart contracts. By monitoring new blocks, applications can check for specific events or transactions within those blocks that are relevant to the operations of a decentralized application (dApp). This is useful for triggering actions based on specific blockchain events, such as updating a user interface or executing follow-up transactions.
3. **Security and Fraud Detection**: By leveraging `eth_newBlockFilter`, security-focused applications can monitor blockchain activity in real-time to detect suspicious activities or potential fraud. For instance, a security application could be set up to watch for unusually large transactions or a high frequency of transactions, which might indicate a potential security threat or fraudulent behavior. This allows for immediate alerts and responses to mitigate risks.

### Code for eth\_newBlockFilter

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
  "method": "eth_newBlockFilter",
  "params": [],
  "id": 67
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
  "method": "eth_newBlockFilter",
  "params": [],
  "id": 67
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

When using the `eth_newBlockFilter` JSON-RPC API BSC method, the following issues may occur:

* Incorrect JSON-RPC version: Ensure that the JSON-RPC version is set to "2.0" as older versions may not support this method correctly.
* Network connectivity issues: If there are network problems, the filter may not be created. Verify your internet connection and ensure that your BSC node is running and accessible.
* Unauthorized access: If you receive an authorization error, check your credentials and ensure that your client has the necessary permissions to interact with the BSC node.
* Resource limitations: Excessive filter creation can lead to resource exhaustion on the node. Consider implementing a cleanup mechanism for unused filters to optimize resource usage.

Using the `eth_newBlockFilter` method in Web3 applications allows developers to efficiently monitor new blocks on the BNB Smart Chain without polling continuously. This method enhances performance by providing a way to receive block notifications, enabling real-time updates and more responsive decentralized applications.

### Conclusion

The `eth_newBlockFilter` method in JSON-RPC is a useful tool for developers working with blockchain networks like BSC (BNB Smart Chain). It allows for the creation of a filter to listen for new blocks, enabling efficient monitoring and interaction with the blockchain. By utilizing `eth_newBlockFilter`, developers can streamline their applications to respond to new block events in real-time, enhancing the responsiveness and functionality of their solutions on platforms like BSC.
