---
description: >-
  eth_unsubscribe method in JSON-RPC API Interface allows users to stop receiving updates for a specific subscription in the BSC protocol.
---

# eth_unsubscribe

{% hint style="success" %}
The RPC method eth_unsubscribe on BSC unsubscribes from a previously subscribed event or data feed, stopping notifications or updates.&#x20;
{% endhint %}

The eth_unsubscribe Web3 method is a part of the JSON-RPC API that enables users to terminate an active subscription within the Binance Smart Chain (BSC) protocol. This method is essential for managing network resources effectively by allowing clients to stop receiving updates or notifications for a specific topic they previously subscribed to. By using the eth_unsubscribe RPC protocol, developers can enhance application performance and reduce unnecessary data traffic. It requires the subscription ID as a parameter, ensuring precise control over active subscriptions. This method is crucial for maintaining efficient and scalable blockchain interactions, providing a streamlined approach to subscription management in decentralized applications.

### Supported Networks

The eth_unsubscribe REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_unsubscribe method needs to be executed. Do not highlight the method name or enclose it in quotation marks.

- **Parameter**: Subscription ID
  - **Required/Optional**: Required
  - **Type**: String
  - **Description**: The identifier of the subscription to be terminated.
  - **Default/Supported Values**: A valid subscription ID, such as "0xe5af64ddfd365b4632988c5935cfedb7".

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_unsubscribe

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {"jsonrpc": "2.0",
"method": "eth_unsubscribe",
"params": ["0xe5af64ddfd365b4632988c5935cfedb7"],
"id": "getblock.io"}
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
    "message": "subscription not found"
  }
}

```

### Body Parameters

Here is the list of body parameters for eth_unsubscribe method:

1. **jsonrpc**: This parameter indicates the version of the JSON-RPC protocol being used. In this case, it is "2.0".

2. **id**: This is a unique identifier for the request. It is used to match the response with the request that it is replying to. In this example, the id is "getblock.io".

3. **error**: This parameter contains details about any error that occurred during the processing of the request. It includes:
   - **code**: A numeric error code that indicates the type of error. For example, -32000 is a common code for server errors.
   - **message**: A brief description of the error. In this case, the message is "subscription not found", indicating that the specified subscription could not be located.

### Use Cases

Here are some use-cases for eth_unsubscribe method in Web3 programming:

1. **Resource Management**: In Web3 applications, especially those that involve real-time data updates from the Ethereum blockchain, developers often subscribe to specific events or data streams using the eth_subscribe method. Over time, maintaining these subscriptions can consume significant resources, both in terms of network bandwidth and computational power. By using the eth_unsubscribe method, developers can efficiently manage resources by terminating subscriptions that are no longer needed, ensuring that the application remains performant and responsive.

2. **Dynamic Application Behavior**: Many decentralized applications (dApps) require dynamic behavior based on user interactions or changes in application state. For instance, a dApp might initially subscribe to certain blockchain events to display live data to the user. However, if the user navigates away from that section of the application, it becomes unnecessary to continue receiving those updates. The eth_unsubscribe method allows developers to dynamically adjust the active subscriptions based on the current context or user actions, improving the overall user experience.

3. **Security and Privacy**: Continuous subscriptions to blockchain data can sometimes lead to unnecessary exposure of application logic or user behavior patterns. By strategically unsubscribing from events when they are no longer relevant, developers can enhance the privacy and security of their applications. This reduces the risk of potential data leaks or vulnerabilities that could be exploited by malicious actors monitoring subscription activities.

### Code for eth_unsubscribe

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
    "message": "subscription not found"
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
When using the eth_unsubscribe JSON-RPC API BSC method, the following issues may occur:  
- Invalid subscription ID: If the subscription ID provided is incorrect or does not exist, the method will fail to unsubscribe. Ensure that the subscription ID is accurate and corresponds to an active subscription.  
- Network connectivity issues: Unstable network connections can cause the unsubscribe request to fail. Verify your network connection and try resending the request if necessary.  
- Server-side limitations: Some nodes may impose limits on the number of concurrent subscriptions, which can affect the ability to manage subscriptions effectively. Consider using a different node or service provider if you encounter such limitations.  
- JSON-RPC version mismatch: Using an outdated or incompatible JSON-RPC version may lead to unexpected errors. Ensure your client is using a compatible version of the JSON-RPC protocol.  

Using eth_unsubscribe in Web3 applications allows developers to efficiently manage and terminate active subscriptions, reducing unnecessary data flow and optimizing resource usage. This method is crucial for maintaining application performance and ensuring that only relevant data is processed, ultimately enhancing the user experience.

### conclusion

The eth_unsubscribe JSON-RPC method is used to cancel a previously established subscription on networks like Ethereum and BSC. By invoking this method, developers can stop receiving updates or notifications related to specific events or data streams, optimizing resource usage and improving application performance.
