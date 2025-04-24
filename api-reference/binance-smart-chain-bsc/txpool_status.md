---
description: >-
  Access txpool_status via JSON-RPC API Interface to monitor pending and queued transactions on the BSC network.
---

# txpool_status

{% hint style="success" %}
The RPC txpool_status for BSC provides information about the number of pending and queued transactions in the transaction pool, aiding in network congestion analysis.&#x20;
{% endhint %}

The txpool_status Web3 method is a key component of the BSC protocol, providing developers with real-time insights into the transaction pool. By utilizing the txpool_status RPC protocol, users can efficiently track the number of pending and queued transactions within the Binance Smart Chain network. This method returns detailed information about the current state of the transaction pool, helping developers and users to manage transaction flows effectively. The JSON-RPC API Interface ensures seamless integration and communication with the blockchain, allowing for precise monitoring and optimization of transaction processing. Whether you're developing applications or monitoring network activity, txpool_status offers a reliable and straightforward way to access crucial transaction data.

### Supported Networks

The txpool_status REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

None: This method does not require any parameters.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using txpool_status

#### Request

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

### Response


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

### Body Parameters

Here is the list of body parameters for txpool_status method:

1. `jsonrpc`: The version of the JSON-RPC protocol, typically "2.0".
2. `id`: A unique identifier for the request, used to match responses to requests.
3. `result`: An object containing the status of the transaction pool.
   - `pending`: The number of transactions that are pending in the pool, represented in hexadecimal format.
   - `queued`: The number of transactions that are queued in the pool, also represented in hexadecimal format.

### Use Cases

Here are some use-cases for txpool_status method in Web3 programming:

1. **Monitoring Transaction Backlog**: This method can be utilized to monitor the number of pending and queued transactions in the transaction pool. By checking the status of the transaction pool, developers can assess network congestion and optimize their applications to handle varying levels of transaction throughput. This is particularly useful for applications that rely on timely transaction processing, such as decentralized exchanges or real-time bidding platforms.

2. **Optimizing Gas Fees**: Developers can use this method to determine the current state of the transaction pool and adjust gas fees accordingly. By understanding the number of transactions waiting to be processed, developers can advise users on appropriate gas fees to ensure their transactions are processed in a timely manner, thereby enhancing the user experience and reducing costs during periods of high network activity.

3. **Debugging and Performance Tuning**: For developers and system administrators, the method provides valuable insights into the transaction pool, aiding in debugging and performance tuning of blockchain applications. By analyzing the transaction pool status, developers can identify bottlenecks or inefficiencies in how transactions are being handled, allowing them to make necessary adjustments to improve the overall performance and reliability of their applications.

### Code for txpool_status

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
{% endtabs %}

## Common Errors

Common Errors  
When using the txpool_status JSON-RPC API BSC method, the following issues may occur:  
- Incorrect JSON structure: Ensure that the JSON-RPC request is properly formatted with all required fields, such as "method," "params," "id," and "jsonrpc," to avoid parsing errors.  
- Network connectivity issues: If the request fails to reach the BSC node, verify your network connection and ensure that the node URL is correctly specified and accessible.  
- Unauthorized access: If you receive an authentication error, check that you have the necessary permissions and that any required API keys or credentials are correctly configured.  
- Outdated client software: Ensure that your BSC client is up to date, as using an outdated version may lead to compatibility issues with the latest network features and API methods.  

The txpool_status method is a valuable tool in Web3 applications, providing insights into the current state of the transaction pool on the BSC network. By monitoring the pending and queued transactions, developers can optimize transaction handling and improve the efficiency of their decentralized applications.

### conclusion

The txpool_status JSON-RPC method is used to retrieve the status of the transaction pool on the Binance Smart Chain (BSC). This method provides insights into the current state of pending and queued transactions, helping developers and users understand the network's transaction processing capabilities. By utilizing txpool_status on BSC, one can effectively monitor and manage transaction throughput and network congestion.
