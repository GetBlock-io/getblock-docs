---
description: >-
  Create a new pending transaction filter using eth_newPendingTransactionFilter in the JSON-RPC API Interface for the BSC protocol.
---

# eth_newPendingTransactionFilter

{% hint style="success" %}
Creates a filter to listen for new pending transactions in the Binance Smart Chain, allowing applications to react to transactions before they're confirmed.&#x20;
{% endhint %}

The eth_newPendingTransactionFilter Web3 method allows developers to create a filter for monitoring new pending transactions in the Binance Smart Chain (BSC) network. By utilizing the eth_newPendingTransactionFilter RPC protocol, users can efficiently track transactions that are yet to be mined, providing a real-time view of network activity. This method is particularly useful for applications that require immediate updates on transaction status or need to respond to transaction events promptly. The filter ID returned by this method can be used with other RPC calls to retrieve and manage pending transaction data. Designed for seamless integration, this function is essential for developers working with the JSON-RPC API Interface to enhance their blockchain applications.

### Supported Networks

The eth_newPendingTransactionFilter REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

None: This method does not require any parameters.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_newPendingTransactionFilter

#### Request

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

### Response


```json

{
  "jsonrpc": "2.0",
  "id": 67,
  "result": "0x746380cef6b04f8ab8c53a309ce8c98d"
}

```

### Body Parameters

Here is the list of body parameters for eth_newPendingTransactionFilter method:

1. `jsonrpc`: The version of the JSON-RPC protocol being used, typically "2.0".
2. `id`: A unique identifier for the request, which can be any number or string. In this case, it is 67.
3. `result`: The response from the method, which is a unique identifier for the newly created filter. In this example, it is "0x746380cef6b04f8ab8c53a309ce8c98d".

### Use Cases

Here are some use-cases for eth_newPendingTransactionFilter method:

1. **Real-time Transaction Monitoring**: This method can be used to keep track of all pending transactions in real-time. Developers can set up a filter to receive notifications whenever a new transaction is added to the pending state. This is particularly useful for applications that need to display live transaction data or for monitoring transaction activity on the network for analytics purposes.

2. **Building Alert Systems**: By utilizing this method, developers can create alert systems that notify users or administrators of specific transaction patterns or activities. For instance, if a transaction meets certain criteria, such as a high gas price or a particular sender address, an alert can be triggered to inform relevant parties. This can be helpful for security monitoring or for users who want to be informed of transactions involving their accounts.

3. **Optimizing Gas Usage**: Developers can use this method to analyze pending transactions and adjust their own transaction strategies accordingly. By understanding the current state of pending transactions, they can decide whether to increase the gas price to ensure quicker inclusion in a block or to wait for network congestion to decrease. This can lead to more efficient use of gas and potentially lower transaction costs.

### Code for eth_newPendingTransactionFilter

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
{% endtabs %}

## Common Errors

Common Errors  
When using the eth_newPendingTransactionFilter JSON-RPC API BSC method, the following issues may occur:  
- Filter not updating: If the filter does not seem to capture new pending transactions, ensure that the node you're connected to is fully synced with the network. Regularly check the node's synchronization status and restart it if necessary.  
- Invalid response format: Occasionally, you might receive responses that are not properly formatted. Verify that your client library correctly parses JSON-RPC responses and that there are no network issues causing incomplete data transfers.  
- Filter ID not found: If you receive an error indicating that the filter ID is not found, double-check that the filter has not been automatically removed by the node due to inactivity. Consider setting up a mechanism to refresh or recreate filters periodically.  
- Node compatibility issues: Some nodes might not fully support all JSON-RPC methods. Ensure that your node is running a version that supports eth_newPendingTransactionFilter and consider upgrading the node software if necessary.

Utilizing the eth_newPendingTransactionFilter method in Web3 applications allows developers to efficiently monitor pending transactions on the BSC network. This can be particularly beneficial for applications that need real-time updates on transaction statuses, enabling quick responses to network activity. By leveraging this method, developers can enhance the interactivity and responsiveness of their blockchain-based applications.

### conclusion

The eth_newPendingTransactionFilter method in JSON-RPC is a valuable tool for developers working on platforms like BSC. It allows them to monitor pending transactions efficiently. By leveraging eth_newPendingTransactionFilter, developers can enhance their applications' responsiveness to real-time transaction data on the blockchain.
