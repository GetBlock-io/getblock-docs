---
description: >-
  eth_newFilter in the JSON-RPC API Interface creates a filter for logs in the
  BSC protocol, enabling efficient event monitoring and data retrieval.
---

# eth\_newFilter - Binance Smart Chain

{% hint style="success" %}
The RPC method creates a filter for monitoring specific blockchain events or logs on the Binance Smart Chain, enabling efficient event tracking and data retrieval.
{% endhint %}

The `eth_newFilter` method in the BSC protocol allows developers to create a new filter object to listen for specific state changes in the blockchain. Utilizing the `eth_newFilter` Web3 interface, this method is essential for applications that need to monitor logs or events efficiently.

Through the `eth_newFilter` RPC protocol, users can specify parameters such as address, topics, and block range to customize their filter. This enables precise event tracking and is crucial for applications requiring real-time data updates.

### Supported Networks

The `eth_newFilter` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_newFilter` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **fromBlock**
  * **Type**: String
  * **Required**: Optional
  * **Description**: The block number from which to start looking for logs. This can be a specific block number or a string like "latest", "earliest", or "pending".
  * **Default/Supported Values**: Hexadecimal string representing a block number or predefined keywords.
* **toBlock**
  * **Type**: String
  * **Required**: Optional
  * **Description**: The block number at which to stop looking for logs. Similar to `fromBlock`, it can be a specific block number or a string like "latest", "earliest", or "pending".
  * **Default/Supported Values**: Hexadecimal string representing a block number or predefined keywords.
* **address**
  * **Type**: String or Array of Strings
  * **Required**: Optional
  * **Description**: The contract address or a list of addresses from which logs should originate. If not specified, logs from all addresses are considered.
  * **Default/Supported Values**: A single address as a string or an array of addresses.
* **topics**
  * **Type**: Array
  * **Required**: Optional
  * **Description**: An array of strings or nulls representing the topics to filter by. Each topic can also be an array of multiple topics, which means that logs must match one of the topics in the array. If not specified, all topics are considered.
  * **Default/Supported Values**: An array of strings or nulls, or an array of arrays for multiple topics.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_newFilter` :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_newFilter",
  "params": [{
    "fromBlock": "0xe20360",
    "toBlock": "0xe20411",
    "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
    "topics": []
  }],
  "id": 1
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by `eth_newFilter` upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0xfb997524907ce408eed52d23df0ffcb9"
}

```

### Body Parameters

Here is the list of body parameters for the `eth_newFilter` method:

1. **jsonrpc**: The version of the JSON-RPC protocol. For Ethereum, this is typically "2.0".
2. **id**: A unique identifier for the request. This is used to match the response with the request.
3. **result**: The ID of the newly created filter, which can be used to retrieve logs or uninstall the filter.

### Use Cases

Here are some use-cases for the `eth_newFilter` method:

1. **Monitoring Contract Events**: The `eth_newFilter` method is commonly used to monitor specific events emitted by a smart contract. By specifying a contract address and relevant topics, developers can track events such as token transfers, approvals, or any custom event defined within a smart contract. This is particularly useful for applications that need to respond to blockchain events in real-time, such as updating user interfaces or triggering off-chain processes.
2. **Building Notification Systems**: Another use case for the `eth_newFilter` method is in constructing notification systems that alert users when certain blockchain events occur. For instance, a decentralized application (dApp) could use this method to notify users when they receive a new token or when a transaction they initiated is confirmed. By continuously polling the filter, the application can efficiently detect and react to changes on the blockchain.
3. **Data Analysis and Auditing**: Developers and analysts can use `eth_newFilter` to gather historical data for analysis or auditing purposes. By setting a range of blocks to monitor, they can collect event logs over a specific time period, which can then be used to analyze trends, verify transactions, or audit the activity of a smart contract. This capability is crucial for ensuring transparency and accountability in blockchain-based systems.

### Code for eth\_newFilter

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
  "method": "eth_newFilter",
  "params": [{
    "fromBlock": "0xe20360",
    "toBlock": "0xe20411",
    "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
    "topics": []
  }],
  "id": 1
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
  "method": "eth_newFilter",
  "params": [{
    "fromBlock": "0xe20360",
    "toBlock": "0xe20411",
    "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
    "topics": []
  }],
  "id": 1
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

When using the `eth_newFilter` JSON-RPC API BSC method, the following issues may occur:

* Incorrect block range: Specifying a `fromBlock` or `toBlock` that is not valid or is out of sync with the network can lead to errors. Ensure that the block numbers are within the current blockchain range and are properly formatted in hexadecimal.
* Invalid address format: Providing an incorrect or improperly formatted contract address can cause the filter to fail. Double-check that the address is a valid Ethereum address and is in the correct format.
* Empty topics array: While an empty `topics` array is acceptable, ensure that this is intentional. If specific events are being targeted, populate the `topics` array with the correct event signatures.
* Network connectivity issues: If the node is not properly connected to the BSC network, the filter may not function as expected. Verify network connectivity and node synchronization status.

Using the `eth_newFilter` method in Web3 applications provides developers with a powerful tool to monitor blockchain events efficiently. It allows for real-time tracking of specific transactions or contract events, enabling responsive and dynamic application behavior. By leveraging this method, developers can enhance the interactivity and responsiveness of their decentralized applications.

### Conclusion

The `eth_newFilter` JSON-RPC method is a powerful tool for developers working with blockchain networks like Ethereum and BSC. It allows users to create a filter for monitoring specific events or transactions within a defined block range or associated with a particular address. By leveraging `eth_newFilter`, developers can efficiently track on-chain activities and respond to them programmatically, enhancing the capabilities of decentralized applications.
