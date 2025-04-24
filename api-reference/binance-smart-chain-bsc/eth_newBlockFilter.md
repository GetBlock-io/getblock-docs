---
description: >-
  eth_newBlockFilter in JSON-RPC API Interface enables real-time BSC block notifications for developers.
---

# eth_newBlockFilter

{% hint style="success" %}
The RPC method creates a filter to receive notifications for new blocks on the BSC, enabling real-time monitoring of blockchain activity.&#x20;
{% endhint %}

The eth_newBlockFilter Web3 method is a key component of the BSC protocol, offering developers a streamlined way to receive notifications about new blocks. By utilizing the eth_newBlockFilter RPC protocol, users can create a filter object that listens for new block events, ensuring they stay updated with the latest blockchain data. This method is particularly useful for applications that require real-time block information, such as monitoring services or decentralized applications that react to new block confirmations. The eth_newBlockFilter method enhances the efficiency and responsiveness of applications interacting with the BSC network, providing a reliable and straightforward mechanism to track blockchain activity through the JSON-RPC API.

### Supported Networks

The eth_newBlockFilter REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

None: This method does not require any parameters.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_newBlockFilter

#### Request

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
{% endtabs %}

### Response


```json

{
  "jsonrpc": "2.0",
  "id": 67,
  "result": "0xcb5b0ec347fb06c786ab6f6f3b4bb584"
}

```

### Body Parameters

Here is the list of body parameters for eth_newBlockFilter method:

1. `jsonrpc`: The version of the JSON-RPC protocol being used. Typically, this is "2.0".
2. `id`: A unique identifier for the request. In this example, it is 67.
3. `result`: The filter ID created by the method, which can be used to poll for new blocks. In this example, it is "0xcb5b0ec347fb06c786ab6f6f3b4bb584".

### Use Cases

Here are some use-cases for eth_newBlockFilter method:

1. **Real-time Block Monitoring**: This method is useful for applications that need to monitor the Ethereum blockchain in real-time. By setting up a block filter, developers can receive notifications whenever a new block is added to the blockchain. This is particularly beneficial for services that need to react promptly to changes in the blockchain, such as updating transaction statuses or triggering smart contract functions based on new block data.

2. **Efficient Data Collection**: For developers building analytics tools or dashboards, this method allows for efficient collection of blockchain data without constantly polling the network. By using a block filter, applications can gather relevant block information as soon as it becomes available, reducing the need for redundant network requests and minimizing bandwidth usage.

3. **Automated Event Handling**: In decentralized applications, there may be a need to automate certain actions based on blockchain events. By utilizing this method, developers can automate workflows that depend on the occurrence of new blocks, such as sending alerts, updating databases, or interacting with other decentralized protocols, ensuring that the application remains responsive and up-to-date with the latest blockchain state.

### Code for eth_newBlockFilter

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
  "result": "0xcb5b0ec347fb06c786ab6f6f3b4bb584"
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
When using the eth_newBlockFilter JSON-RPC API BSC method, the following issues may occur:  
- Incorrect JSON Structure: If the JSON request is improperly formatted or contains syntax errors, the server may return a parse error. Ensure that your JSON structure adheres to the correct format and all necessary fields are included.  
- Network Connectivity Issues: If there are connectivity problems between your client and the BSC node, you might experience timeouts or failed requests. Verify your network connection and ensure that the node endpoint is reachable.  
- Invalid Parameters: Although eth_newBlockFilter does not require parameters, providing unexpected parameters might lead to an invalid request error. Double-check that your request does not include unnecessary fields.  
- Unauthorized Access: If the node requires authentication and your request lacks proper credentials, it could be rejected. Ensure you have the necessary permissions and include any required authentication details.

Using the eth_newBlockFilter method in Web3 applications is beneficial as it allows developers to efficiently monitor new blocks on the BSC network without polling for updates continuously. This can lead to more responsive applications and reduced network overhead, enhancing the overall user experience.

### conclusion

The eth_newBlockFilter method in JSON-RPC is a useful tool for developers working with blockchain networks like Ethereum and Binance Smart Chain (BSC). It allows users to create a filter that listens for new blocks, enabling real-time updates and efficient monitoring of the blockchain's state. By leveraging eth_newBlockFilter, developers can enhance their applications with timely data and improve overall responsiveness.
