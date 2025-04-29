---
description: >-
  Access blockchain event logs using eth_getLogs via the JSON-RPC API Interface
  in BSC, enabling efficient data retrieval for developers.
---

# eth\_getLogs - BNB Smart Chain

{% hint style="success" %}
The RPC eth\_getLogs retrieves event logs from the BNB Smart Chain, aiding in tracking contract events and filtering blockchain data.
{% endhint %}

The `eth_getLogs` method in the BSC protocol is a part of the `eth_getLogs` Web3 and `eth_getLogs` RPC protocol, designed to retrieve event logs from the blockchain. This method allows users to filter logs based on specific criteria such as block range, contract address, and topics, providing a powerful tool for monitoring and analyzing blockchain events.

Utilizing `eth_getLogs`, developers can efficiently access historical and real-time data for smart contract interactions. The method returns an array of log objects, each containing details like block number, transaction hash, and data payload. This functionality is crucial for applications that need to track contract events or audit blockchain activities.

### Supported Networks

The `eth_getLogs` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_getLogs` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **address** (optional)
  * **Type**: string
  * **Description**: The contract address from which logs should be retrieved.
  * **Default/Supported Values**: Any valid Ethereum address. If omitted, logs from all addresses are returned.

Note: The `eth_getLogs` method can accept additional parameters such as `fromBlock`, `toBlock`, `topics`, etc., which are not present in this request.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_getLogs` :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getLogs",
  "params": [{
    "address": "0xdAC17F958D2ee523a2206206994597C13D831ec7"
  }],
  "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by `eth_getLogs` upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": []
}

```

### Body Parameters

Here is the list of body parameters for `eth_getLogs` method:

1. **fromBlock**: (optional) The block number, block hash, or the string "earliest", "latest" (default), or "pending", from which to start looking for logs.
2. **toBlock**: (optional) The block number, block hash, or the string "latest" (default) or "pending", up to which to look for logs.
3. **address**: (optional) A single address or an array of addresses from which logs should originate.
4. **topics**: (optional) An array of values which must appear in the log entries. Each topic can also be null, which matches any topic.
5. **blockhash**: (optional) With the addition of EIP-234, blockhash can be used to restrict the logs to a specific block. If blockhash is present, fromBlock and toBlock are ignored.

### Use Cases

Here are some use-cases for `eth_getLogs` method:

1. **Event Monitoring**: Developers often use the `eth_getLogs` method to monitor specific events that are emitted by smart contracts. For instance, if you are interested in tracking all transfers of USDT (Tether), you can use this method to filter logs from the Tether contract address. By specifying the event signature in the filter, you can receive notifications whenever a transfer event occurs, allowing you to build applications that react to these events in real-time.
2. **Historical Data Retrieval**: Another common use-case for `eth_getLogs` is to retrieve historical event data from the blockchain. This is particularly useful for applications that need to analyze past transactions or events to generate reports, perform audits, or track the history of a particular asset. By specifying a block range in the parameters, developers can fetch logs from specific periods to gather the necessary historical data.
3. **Debugging and Analysis**: `eth_getLogs` can also be used for debugging purposes. Developers can use it to analyze the logs generated during contract execution to understand how a contract behaves and identify any issues. This can be crucial for optimizing smart contract performance and ensuring that they function as intended. By examining the logs, developers can gain insights into the state changes and interactions that occur within their contracts.

### Code for eth\_getLogs

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
  "method": "eth_getLogs",
  "params": [{
    "address": "0xdAC17F958D2ee523a2206206994597C13D831ec7"
  }],
  "id": "getblock.io"
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
  "method": "eth_getLogs",
  "params": [{
    "address": "0xdAC17F958D2ee523a2206206994597C13D831ec7"
  }],
  "id": "getblock.io"
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

When using the `eth_getLogs` JSON-RPC API BSC method, the following issues may occur:

* **Incorrect Address Format**: If the address format is incorrect or not checksummed, the request may fail. Ensure the address is in a valid Ethereum checksum format by using a tool like Etherscan's address converter.
* **Exceeding Log Limits**: Requests that specify too wide a block range might exceed the log limits, resulting in incomplete data. Narrow down the block range and make multiple requests to retrieve all necessary logs.
* **Node Synchronization Lag**: If the node is not fully synchronized, it might not return the latest logs. Verify that your node is fully synced with the BSC network before making requests.
* **Network Congestion**: High network traffic could delay responses or cause timeouts. Consider implementing retry logic with exponential backoff to handle such scenarios gracefully.

The `eth_getLogs` method is invaluable in Web3 applications as it allows developers to efficiently query blockchain events and filter logs based on specific criteria. This functionality is crucial for building responsive and data-driven decentralized applications that rely on real-time blockchain data.

### Conclusion

The `eth_getLogs` JSON-RPC method is a powerful tool for retrieving event logs from the Ethereum blockchain, allowing users to filter logs by specific contract addresses, such as the one provided for USDT. This method is also applicable to other EVM-compatible chains like BNB Smart Chain (BSC), making it versatile for developers working across different blockchain networks.
