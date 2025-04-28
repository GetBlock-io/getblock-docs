---
description: >-
  Retrieve logs using eth_getFilterLogs via the JSON-RPC API Interface in BSC, offering efficient access to blockchain event data.
---

# eth_getFilterLogs

{% hint style="success" %}
eth_getFilterLogs retrieves logs that match a specified filter on the Binance Smart Chain, primarily used to track events and transactions.&#x20;
{% endhint %}

The `eth_getFilterLogs` method in the BSC protocol is a part of the JSON-RPC API, enabling users to retrieve an array of log objects that match the criteria set by a filter ID. This method is integral to the `eth_getFilterLogs Web3` interface, allowing developers to efficiently access event logs without manually parsing blocks.

In the `eth_getFilterLogs RPC protocol`, the method requires a single parameter, the filter ID, which is obtained from a previous filter creation call such as `eth_newFilter`. By leveraging this method, developers can streamline their log retrieval processes, ensuring precise and relevant data extraction for their decentralized applications.

## Supported Networks

The eth_getFilterLogs JSON-RPC API method supports the following network types:
- **Mainnet**
- **Testnet**

## Parameters

Here is the list of parameters `eth_getFilterLogs` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

- **Filter ID**
  - **Type:** String
  - **Description:** The ID of the filter for which to retrieve logs. This ID is obtained from a previous call to `eth_newFilter` or similar methods.
  - **Required:** Yes
  - **Default/Supported Values:** A valid filter ID in hexadecimal format.

# Request Example

##### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```


#### Request

Here’s a sample cURL request using eth_getFilterLogs :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getFilterLogs",
  "params": ["0xfba02b32cc0fd31639b68144ebc59fd2"],
  "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

#### Response

Below is a sample JSON response returned by eth_getFilterLogs upon a successful call:

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

## Body Parameters

Here is the list of body parameters for the `eth_getFilterLogs` method:

1. **jsonrpc**: Specifies the version of the JSON-RPC protocol. It should be "2.0".
2. **id**: A unique identifier for the request. It can be any string or number, and is used to match the response with the request.
3. **method**: The name of the method being called, which in this case is `eth_getFilterLogs`.
4. **params**: An array containing the filter ID for which logs are being requested. This ID is obtained from a previous call to the `eth_newFilter` method.

## Use Cases

Here are some use-cases for `eth_getFilterLogs` method:

1. **Monitoring Contract Events**: One of the primary use-cases for the `eth_getFilterLogs` method is to monitor and retrieve logs for specific events emitted by smart contracts. Developers can create a filter to listen for particular events and use this method to fetch all logs that match the filter criteria. This is especially useful for applications that need to react to on-chain events, such as updating user interfaces in real-time when a transaction is confirmed or when specific conditions are met within a decentralized application.

2. **Historical Data Analysis**: `eth_getFilterLogs` can be employed to gather historical log data for analysis. By setting up a filter with the appropriate parameters, developers can retrieve logs from past blocks, which can then be used for data analytics, auditing, or generating reports. This capability is crucial for understanding the behavior of smart contracts over time and for verifying that they have operated as expected.

3. **Debugging and Testing**: During the development and testing phases of a smart contract, developers can use `eth_getFilterLogs` to debug and verify that events are being emitted correctly. By fetching logs for specific transactions or blocks, developers can ensure that their contracts are functioning as intended and that event data is being recorded accurately. This is an essential step in the development process to catch and fix issues before deploying contracts to the mainnet.

## Code for eth_getFilterLogs

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

## Common Errors

When using the `eth_getFilterLogs` JSON-RPC API BSC method, the following issues may occur:
- **Invalid Filter ID**: If the filter ID provided is incorrect or expired, the method will return an error. Ensure the filter ID is valid and active by checking the filter creation time and refreshing it if necessary.
- **Network Latency**: Due to network congestion, responses might be delayed, leading to timeouts. Consider implementing retry logic with exponential backoff to handle these delays gracefully.
- **Permission Denied**: Access to logs might be restricted based on node permissions. Verify that your node configuration allows for reading logs and adjust the settings if needed.
- **Malformed JSON Request**: Incorrect JSON formatting can lead to parsing errors. Double-check the JSON structure for syntax errors, such as missing commas or brackets, before sending the request.

The `eth_getFilterLogs` method is highly beneficial in Web3 applications, allowing developers to efficiently retrieve event logs based on specific filter criteria. This functionality is crucial for tracking blockchain events and building responsive, event-driven applications that react to on-chain activities in real-time.

## Conclusion

The `eth_getFilterLogs` method in the JSON-RPC API is a powerful tool for retrieving logs from the Ethereum blockchain or compatible networks like Binance Smart Chain (BSC). By specifying a filter identifier, users can efficiently access event data that meets certain criteria. This capability is crucial for developers and analysts who need to monitor specific blockchain activities in real-time.
