---
description: >-
  Use eth_newFilter in the JSON-RPC API Interface to create new filters for event logs on the BSC protocol.
---

# eth_newFilter

{% hint style="success" %}
The RPC method creates a filter to monitor blockchain events, such as logs and smart contract interactions, on BSC, enabling real-time event tracking and notifications.&#x20;
{% endhint %}

The eth_newFilter Web3 method in the BSC protocol allows developers to create new filters for monitoring event logs efficiently. By utilizing the eth_newFilter RPC protocol, users can specify parameters such as address, topics, and block range to tailor the filter to their needs. This method is essential for applications requiring real-time data tracking and event-driven programming. Once a filter is created, it returns a filter ID, which can be used with other JSON-RPC methods to poll for changes or uninstall the filter. This functionality is crucial for developers looking to build responsive and data-driven applications on the Binance Smart Chain, ensuring they can capture and react to blockchain events as they occur.

### Supported Networks

The eth_newFilter REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_newFilter method needs to be executed. Do not highlight the method name or enclose it in quotation marks.

- **fromBlock**
  - **Type**: String (Hexadecimal)
  - **Description**: The block number from which to start looking for logs. This parameter specifies the starting block for the filter.
  - **Optional/Required**: Optional
  - **Default/Supported Values**: If not specified, the default is "latest".

- **toBlock**
  - **Type**: String (Hexadecimal)
  - **Description**: The block number up to which to look for logs. This parameter specifies the ending block for the filter.
  - **Optional/Required**: Optional
  - **Default/Supported Values**: If not specified, the default is "latest".

- **address**
  - **Type**: String
  - **Description**: The contract address from which logs should originate. This parameter filters logs based on the specified address.
  - **Optional/Required**: Optional
  - **Default/Supported Values**: Can be a single address or an array of addresses.

- **topics**
  - **Type**: Array
  - **Description**: The topics to filter logs by. Each topic can also be an array of multiple topics.
  - **Optional/Required**: Optional
  - **Default/Supported Values**: An empty array indicates no filtering by topics.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_newFilter

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_newFilter",
  "params": [{
    "fromBlock": "0xe20360",
    "toBlock": "0xe20411",
    "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
    "topics": []
  }],
  "id": 1
}
```
{% endtab %}
{% endtabs %}

### Response


```json

{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0xfb997524907ce408eed52d23df0ffcb9"
}

```

### Body Parameters

Here is the list of body parameters for eth_newFilter method:

1. **fromBlock**: (optional) The block number from which to start looking for logs. It can be specified as a hex string or as "latest", "earliest", or "pending".

2. **toBlock**: (optional) The block number up to which to look for logs. Similar to fromBlock, it can be specified as a hex string or as "latest", "earliest", or "pending".

3. **address**: (optional) An address or a list of addresses from which logs should originate.

4. **topics**: (optional) A list of topics to filter by. Each topic can be a single topic or an array of topics.

5. **blockHash**: (optional) With blockHash set, the filter restricts logs to the specified block. This field is mutually exclusive with fromBlock and toBlock.

### Use Cases

Here are some use-cases for eth_newFilter method:

1. **Monitoring Smart Contract Events**: Developers can use this method to monitor specific events emitted by a smart contract. By setting up a filter with the contract's address and relevant event topics, applications can listen for and respond to certain actions, such as token transfers or state changes, in real-time. This is particularly useful in decentralized applications (dApps) that need to update their UI or trigger other processes based on blockchain activity.

2. **Tracking Transactions in a Block Range**: The method allows developers to specify a range of blocks to watch for transactions involving a particular address. This can be useful for auditing purposes, where a user or application needs to verify all transactions that occurred within a specific timeframe. By setting the fromBlock and toBlock parameters, developers can efficiently track and analyze blockchain activity over a defined period.

3. **Building Notification Systems**: By leveraging this method, developers can create notification systems that alert users or systems when certain conditions are met on the blockchain. For example, a dApp could notify a user when they receive a payment or when a specific event related to their account occurs. This enhances user engagement and provides timely updates without requiring users to manually check the blockchain.

### Code for eth_newFilter

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
  "result": "0xfb997524907ce408eed52d23df0ffcb9"
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
When using the eth_newFilter JSON-RPC API BSC method, the following issues may occur:  
- Incorrect block range: Specifying a block range that does not exist or is out of sync with the network can lead to empty results. Ensure your node is fully synced and the block numbers are accurate.  
- Invalid address format: Providing an incorrect smart contract address format can cause the filter to fail. Double-check the address format to ensure it is a valid hexadecimal string.  
- Missing topics array: Although an empty topics array is valid, if filtering by specific events is required, ensure the topics array is properly structured with the correct event signatures.  
- Network latency: High network latency can delay filter updates and result in missed logs. Optimize your network setup or consider using a more robust node provider to mitigate this issue.  

Using the eth_newFilter method in Web3 applications allows developers to efficiently monitor and react to blockchain events by setting up filters for specific smart contract interactions. This capability enhances the responsiveness and interactivity of decentralized applications, providing users with real-time updates and a seamless experience.

### conclusion

The eth_newFilter method in JSON-RPC is used to create a filter object for monitoring specific events on the Ethereum blockchain, and it can also be utilized in similar ecosystems like BSC (Binance Smart Chain). By specifying parameters such as fromBlock, toBlock, and address, users can efficiently track events and transactions within a defined range. This method is a crucial tool for developers looking to automate event listening and streamline blockchain interactions.
