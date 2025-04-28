---
description: >-
  Create a pending transaction filter using eth_newPendingTransactionFilter in the JSON-RPC API Interface for real-time BSC network monitoring.
---

# eth_newPendingTransactionFilter

{% hint style="success" %}
The RPC method creates a filter to monitor new pending transactions on the Binance Smart Chain, enabling real-time tracking of transaction activity.&#x20;
{% endhint %}

The `eth_newPendingTransactionFilter` method in the BSC protocol is a JSON-RPC API call used to create a filter that listens for new pending transactions. This method is part of the `eth_newPendingTransactionFilter Web3` suite, enabling developers to monitor transactions that have been submitted but not yet mined into a block.

Utilizing the `eth_newPendingTransactionFilter RPC protocol`, developers can efficiently track transaction activity in real-time. This method returns a filter ID, which can be used with other API calls to fetch or remove pending transaction data, enhancing the ability to build responsive and interactive blockchain applications.

## Supported Networks

The eth_newPendingTransactionFilter JSON-RPC API method supports the following network types:
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

Here’s a sample cURL request using eth_newPendingTransactionFilter :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_newPendingTransactionFilter",
  "params": [],
  "id": 67
}
```
{% endtab %}
{% endtabs %}

#### Response

Below is a sample JSON response returned by eth_newPendingTransactionFilter upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": 67,
  "result": "0x746380cef6b04f8ab8c53a309ce8c98d"
}

```

## Body Parameters

Here is the list of body parameters for the `eth_newPendingTransactionFilter` method:

1. **jsonrpc**: Indicates the version of the JSON-RPC protocol being used. In this case, it is `"2.0"`.

2. **id**: A unique identifier for the request. It is used to match the response with the request. In this example, the ID is `67`.

3. **result**: The result of the `eth_newPendingTransactionFilter` method call, which is a filter ID. This ID is used to identify the filter and retrieve pending transactions. In this example, the result is `"0x746380cef6b04f8ab8c53a309ce8c98d"`.

## Use Cases

Here are some use-cases for `eth_newPendingTransactionFilter` method:

1. **Real-time Transaction Monitoring**: One of the primary use-cases for the `eth_newPendingTransactionFilter` method is to monitor pending transactions in real-time. Developers and blockchain enthusiasts can use this method to create applications that track transactions as they are broadcasted to the Ethereum network but before they are included in a block. This can be particularly useful for building alert systems that notify users about transactions involving specific addresses or smart contracts.

2. **Network Analysis and Statistics**: By using `eth_newPendingTransactionFilter`, developers can gather data on the number and types of transactions being submitted to the network. This information can be used for analytical purposes, such as understanding network congestion, transaction fee trends, or identifying patterns in transaction types. Such insights can help in optimizing smart contract performance or improving user experience by adjusting transaction strategies.

3. **Front-running Prevention**: In decentralized finance (DeFi) and other blockchain applications, front-running is a concern where malicious actors attempt to gain an advantage by executing a transaction ahead of a known pending transaction. Developers can use `eth_newPendingTransactionFilter` to detect and analyze pending transactions, allowing them to implement strategies to mitigate front-running risks, such as adjusting transaction parameters or implementing transaction ordering mechanisms.

## Code for eth_newPendingTransactionFilter

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
  "result": "0x746380cef6b04f8ab8c53a309ce8c98d"
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
  "result": "0x746380cef6b04f8ab8c53a309ce8c98d"
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

When using the `eth_newPendingTransactionFilter` JSON-RPC API BSC method, the following issues may occur:
- Network Latency: High network latency can lead to delayed notifications of new pending transactions. To mitigate this, ensure a stable and fast internet connection and consider using a WebSocket connection for real-time updates.
- Filter Limitations: The filter may not capture all pending transactions if the node is under heavy load. To address this, periodically refresh the filter and handle any missed transactions by querying the transaction pool directly.
- Node Synchronization: If the node is not fully synchronized with the network, it may provide incomplete or outdated transaction data. Ensure your node is fully synced before deploying filters to receive accurate results.

Using the `eth_newPendingTransactionFilter` method in Web3 applications allows developers to efficiently monitor the transaction pool for new pending transactions. This capability is crucial for applications that need to react to transaction submissions in real-time, such as automated trading bots or transaction monitoring tools, providing a responsive and dynamic user experience.

## Conclusion

The `eth_newPendingTransactionFilter` method in JSON-RPC is a crucial tool for developers working on Ethereum and Binance Smart Chain (BSC) networks. It allows them to create a filter to monitor pending transactions efficiently. By leveraging `eth_newPendingTransactionFilter`, developers can stay updated on the transaction pool dynamics and optimize their decentralized applications accordingly.
