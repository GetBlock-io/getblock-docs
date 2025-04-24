---
description: >-
  Retrieve filter changes using the eth_getFilterChanges method in the JSON-RPC API Interface for the BSC protocol.
---

# eth_getFilterChanges

{% hint style="success" %}
Retrieves changes in blockchain logs or pending transactions since the last poll, helping track updates efficiently on Binance Smart Chain.&#x20;
{% endhint %}

The eth_getFilterChanges Web3 method is a key component of the JSON-RPC interface in the BSC protocol, designed to efficiently track and retrieve changes to filters. This method is particularly useful for developers who need to monitor specific blockchain events or transactions without repeatedly polling the entire blockchain. By calling the eth_getFilterChanges RPC protocol, users can obtain an array of event logs or transaction hashes that have occurred since the last call, minimizing unnecessary data transmission and optimizing performance. This functionality is essential for applications requiring real-time updates and is integral to maintaining efficient communication between decentralized applications and the blockchain network.

### Supported Networks

The eth_getFilterChanges REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_getFilterChanges method needs to be executed. Do not highlight the method name or enclose it in quotation marks.

- Filter ID
  - Type: String
  - Description: The ID of the filter for which you want to retrieve changes. This ID is obtained from a previous filter creation call.
  - Required: Yes
  - Default/Supported Values: A valid filter ID string, typically returned by methods like `eth_newFilter` or `eth_newBlockFilter`.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_getFilterChanges

#### Request

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

### Response


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

Here is the list of body parameters for eth_getFilterChanges method:

1. **jsonrpc**: The version of the JSON-RPC protocol. It should be "2.0".
2. **id**: A unique identifier for the request. It can be any string or number, such as "getblock.io".
3. **error**: An object that contains error details if the request fails.
   - **code**: A numeric code representing the error type. For example, -32000 indicates a specific server error.
   - **message**: A descriptive message explaining the error, such as "filter not found".

### Use Cases

Here are some use-cases for eth_getFilterChanges method in Web3 programming:

1. **Monitoring Event Logs**: This method is commonly used to monitor smart contract events on the Ethereum blockchain. Developers can create a filter to listen for specific events emitted by smart contracts. By using eth_getFilterChanges, they can periodically check for new events that match their filter criteria. This is particularly useful for applications that need to respond to on-chain events, such as updating user interfaces or triggering off-chain processes.

2. **Tracking Transactions**: Developers can use this method to track pending transactions in the Ethereum network. By setting up a filter for pending transactions, they can retrieve updates on new transactions that are added to the mempool. This is beneficial for applications that need to provide real-time transaction status updates to users or for monitoring network activity for analytics purposes.

3. **Detecting Changes in Account State**: Another use case is to monitor changes in the state of specific accounts. By creating a filter for account-related changes, developers can use eth_getFilterChanges to detect updates such as balance changes or nonce increments. This is useful for applications that need to maintain an up-to-date view of account states, such as wallets or financial dashboards.

### Code for eth_getFilterChanges

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
{% endtabs %}

## Common Errors

Common Errors  
When using the eth_getFilterChanges JSON-RPC API BSC method, the following issues may occur:  
- Invalid filter ID: If the filter ID is incorrect or has expired, the method will return an error. Ensure the filter ID is valid and has been recently obtained from a successful eth_newFilter call.  
- Empty result set: This may occur if there are no new events or logs since the last call. Verify that the filter parameters are correctly set to capture the desired changes.  
- Network connectivity issues: Intermittent network problems can lead to failed requests. Check your network connection and retry the request if necessary.  
- Exceeded rate limits: Excessive requests in a short time frame may hit rate limits imposed by the provider. Implement request throttling or contact your provider for higher rate limits.  

Using the eth_getFilterChanges method in Web3 applications allows developers to efficiently track changes in the blockchain state without polling for new data continuously. This method helps optimize network usage and enhances application performance by only retrieving new data when it is available.

### conclusion

The eth_getFilterChanges JSON-RPC method is a useful tool for developers working with Ethereum or BSC, as it allows them to efficiently track changes in the blockchain that match a specific filter. By regularly polling this method with the appropriate filter ID, developers can stay updated on new events without needing to re-scan the entire blockchain, enhancing the efficiency of their applications.
