---
description: >-
  Access current transaction pool stats using the txpool_status method via the JSON-RPC API Interface in the BSC protocol.
---

# txpool_status

{% hint style="success" %}
The RPC txpool_status for BSC provides information about the transaction pool's current status, including pending and queued transactions, to help manage network congestion.&#x20;
{% endhint %}

The `txpool_status` method in the BSC protocol provides a concise overview of the transaction pool's current state. As part of the `txpool_status Web3` interface, it returns essential metrics about the pending and queued transactions, aiding developers in monitoring network congestion and transaction flow.

Utilizing the `txpool_status RPC protocol`, this method delivers JSON-RPC formatted data, ensuring seamless integration with existing Web3 applications. By offering real-time insights into the transaction pool, it empowers developers to optimize transaction handling and enhance application performance.

## Supported Networks

The txpool_status JSON-RPC API method supports the following network types:
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

Here’s a sample cURL request using txpool_status :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "method": "txpool_status",
  "params": [],
  "id": 1,
  "jsonrpc": "2.0"
}
```
{% endtab %}
{% endtabs %}

#### Response

Below is a sample JSON response returned by txpool_status upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "pending": "0x40ac",
    "queued": "0xd45"
  }
}

```

## Body Parameters

Here is the list of body parameters for `txpool_status` method:

1. **pending**: This parameter indicates the number of transactions that are currently pending in the transaction pool. In this response, the value is "0x40ac", which is the hexadecimal representation of the pending transactions count.

2. **queued**: This parameter represents the number of transactions that are queued in the transaction pool, waiting to be processed. In this response, the value is "0xd45", which is the hexadecimal representation of the queued transactions count.

## Use Cases

Here are some use-cases for `txpool_status` method:

1. **Monitoring Transaction Backlog**: The `txpool_status` method is useful for developers and network operators to monitor the current state of the transaction pool in an Ethereum node. By using this method, one can retrieve information about the number of pending and queued transactions. This is particularly helpful for applications that need to adapt their behavior based on network congestion, such as dynamically adjusting gas prices to ensure timely transaction processing.

2. **Network Health Analysis**: Another use case for the `txpool_status` method is to analyze the overall health of the Ethereum network. By periodically checking the status of the transaction pool, developers can gather insights into network activity and identify potential bottlenecks or issues. This information can be used to optimize node performance and improve the efficiency of decentralized applications.

3. **Debugging and Development**: During the development and testing of smart contracts or decentralized applications, developers can use the `txpool_status` method to track the flow of transactions through the pool. This can help in identifying issues with transaction submission, such as incorrect gas limits or nonce errors, allowing for more efficient debugging and refinement of the application logic.

## Code for txpool_status

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
  "id": 1,
  "result": {
    "pending": "0x40ac",
    "queued": "0xd45"
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
  "id": 1,
  "result": {
    "pending": "0x40ac",
    "queued": "0xd45"
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

When using the `txpool_status` JSON-RPC API BSC method, the following issues may occur:
- Network Latency: High network latency can lead to delayed responses. Ensure your network connection is stable and consider retrying the request after a short interval.
- Invalid Parameters: Sending incorrect parameters will result in an error. Double-check the method call to ensure all parameters are correctly formatted and appropriate for `txpool_status`.
- Rate Limiting: Exceeding the API rate limits can cause request failures. Implement rate limiting in your application to avoid hitting the API too frequently and consider using exponential backoff strategies for retries.

Using the `txpool_status` method in Web3 applications provides a real-time snapshot of the transaction pool, allowing developers to monitor pending transactions and optimize transaction management strategies. This enhances the application's ability to handle transactions efficiently, ensuring a smoother user experience.

## Conclusion

The `txpool_status` method is a JSON-RPC call used to retrieve the status of the transaction pool on blockchain networks like Binance Smart Chain (BSC). By utilizing this method, users can gain insights into the current state of pending and queued transactions in the network's transaction pool. Understanding the `txpool_status` is essential for developers and users to monitor network congestion and transaction processing on BSC.
