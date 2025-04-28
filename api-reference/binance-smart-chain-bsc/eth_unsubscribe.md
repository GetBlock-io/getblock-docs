---
description: >-
  eth_unsubscribe in the JSON-RPC API Interface allows users to stop receiving
  updates from a subscription in the BSC protocol efficiently.
---

# eth\_unsubscribe - Binance Smart Chain

{% hint style="success" %}
The RPC eth\_unsubscribe for BSC stops active subscriptions to events or data streams, ceasing further updates or notifications.
{% endhint %}

The `eth_unsubscribe` method in the BSC protocol is used to terminate an existing subscription, effectively stopping the delivery of notifications to the client. In the context of `eth_unsubscribe` Web3, this method is crucial for managing resources efficiently by ceasing unnecessary data flow.

Utilizing the `eth_unsubscribe` RPC protocol, clients can send a request with the subscription ID they wish to cancel. This ensures that the client no longer receives updates related to the specified subscription, optimizing network and client performance.

### Supported Networks

The `eth_unsubscribe` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_unsubscribe` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Subscription ID**
  * **Type**: String
  * **Description**: The unique identifier for the subscription that needs to be canceled.
  * **Required**: Yes
  * **Default/Supported Values**: A valid subscription ID that was previously obtained through a subscription method like `eth_subscribe`.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_unsubscribe` :

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

**Response**

Below is a sample JSON response returned by `eth_unsubscribe` upon a successful call:

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

Here is the list of body parameters for the `eth_unsubscribe` method:

1. **jsonrpc**: Specifies the version of the JSON-RPC protocol. Typically set to "2.0".
2. **id**: A unique identifier for the request. It helps in matching the response with the request.
3. **error**: This object contains details about any error that occurred during the execution of the method.
   * **code**: A numeric code representing the specific error. For example, `-32000` indicates a generic server error.
   * **message**: A brief description of the error, such as "subscription not found".

### Use Cases

Here are some use-cases for `eth_unsubscribe` method:

1. **Resource Management**: In Web3 applications, especially those that involve real-time data updates such as dApps or blockchain explorers, developers often subscribe to events or data streams using methods like `eth_subscribe`. Over time, these subscriptions can accumulate and consume resources unnecessarily if they are not managed properly. Using `eth_unsubscribe`, developers can efficiently manage and release these resources by unsubscribing from data streams that are no longer needed. This helps in optimizing the application's performance and reducing unnecessary load on the network.
2. **User Session Management**: In scenarios where user sessions are created in a dApp, developers might subscribe to certain blockchain events that are relevant to a user's activity. When a user logs out or their session ends, it is crucial to clean up these subscriptions to prevent further data being pushed to the application, which is now irrelevant. The `eth_unsubscribe` method allows developers to gracefully terminate these subscriptions, ensuring that the application only listens to events that are pertinent to active user sessions.
3. **Dynamic Subscription Handling**: In applications where the need for specific data changes dynamically based on user interactions or application state, `eth_unsubscribe` can be used to adjust subscriptions on-the-fly. For example, a user might switch between different views or filters in a dApp, each requiring different data streams. By unsubscribing from the old data streams and subscribing to new ones as needed, the application remains responsive and efficient, providing users with a seamless experience.

### Code for eth\_unsubscribe

{% tabs %}
{% tab title="Python" %}
```python

import requests
import json
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {"jsonrpc": "2.0",
"method": "eth_unsubscribe",
"params": ["0xe5af64ddfd365b4632988c5935cfedb7"],
"id": "getblock.io"}

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
const payload = {"jsonrpc": "2.0",
"method": "eth_unsubscribe",
"params": ["0xe5af64ddfd365b4632988c5935cfedb7"],
"id": "getblock.io"};

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

When using the `eth_unsubscribe` JSON-RPC API BSC method, the following issues may occur:

* **Invalid Subscription ID**: If the provided subscription ID is incorrect or no longer valid, the method will fail. Ensure that the subscription ID is active and correctly referenced in your application.
* **Network Latency**: High network latency can cause delays in processing the unsubscribe request. To mitigate this, verify network stability and consider retry mechanisms in your application logic.
* **Permission Denied**: If the node or service provider restricts access to unsubscribe functionality, you may encounter permission errors. Check your permissions and ensure that your API key or user account has the necessary access rights.
* **Node Synchronization Issues**: If the node is not fully synchronized with the network, it may not process the unsubscribe request correctly. Verify node synchronization status and consider switching to a fully synced node if necessary.

Using the `eth_unsubscribe` method in Web3 applications offers the benefit of efficiently managing active subscriptions, reducing unnecessary data traffic, and optimizing resource usage. By properly unsubscribing from events when they are no longer needed, developers can maintain cleaner application states and improve overall performance.

### Conclusion

The `eth_unsubscribe` method in JSON-RPC is used to cancel a previously established subscription, such as those for new blocks or pending transactions, on blockchain networks like Ethereum and Binance Smart Chain (BSC). By using `eth_unsubscribe`, developers can effectively manage active subscriptions and optimize resource usage. This functionality is crucial for maintaining efficient communication between clients and the blockchain network.
