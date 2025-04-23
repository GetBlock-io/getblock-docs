---
description: >-
  Retrieve event logs using the 'eth_getLogs' method in the Tron protocol's
  JSON-RPC API Interface.
---

# eth\_getLogs - TRON

## Description

The 'eth\_getLogs' method in the Tron protocol is a Web3 JSON-RPC API interface used to fetch logs based on filter criteria. This method is integral for developers needing to access event logs from the blockchain, enabling the retrieval of specific transaction details and contract events. By providing parameters such as block range, address, and topics, users can efficiently query and obtain relevant log data. The 'eth\_getLogs' RPC protocol is designed for seamless integration into applications, ensuring developers can easily monitor and analyze blockchain activities. This method is essential for applications requiring detailed event tracking and data analysis within the Tron ecosystem.

## Supported Networks

The eth\_getLogs RPC method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters the `eth_getLogs` method needs to be executed:

* **address** (optional)
  * **Type**: Array of strings
  * **Description**: An array of addresses from which logs should originate. Only logs created from these addresses will be returned.
  * **Default/Supported Values**: Any valid Ethereum address in string format.
* **fromBlock** (optional)
  * **Type**: String
  * **Description**: The block number from which to start looking for logs. This is specified in hexadecimal format.
  * **Default/Supported Values**: Any valid block number in hexadecimal format.
* **toBlock** (optional)
  * **Type**: String
  * **Description**: The block number at which to stop looking for logs. This is specified in hexadecimal format.
  * **Default/Supported Values**: Any valid block number in hexadecimal format.
* **topics** (optional)
  * **Type**: Array of strings or null
  * **Description**: Topics are used to filter logs. Each log entry can have up to 4 topics. The first topic is typically the event signature. The topics parameter allows filtering on the first topic and optionally on the subsequent topics.
  * **Default/Supported Values**: An array of topic strings in hexadecimal format or null. Each topic can also be an array of strings to match any one of the topics in the array.

This request specifies logs from two addresses, a specific block range, and filters based on the first topic and two possible values for the second topic.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using eth\_getLogs

Request

```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' 
--header 'Content-Type: application/json' 
{"jsonrpc": "2.0", "method": "eth_getLogs", "params": [{"address":["cc2e32f2388f0096fae9b055acffd76d4b3e5532","E518C608A37E2A262050E10BE0C9D03C7A0877F3"],"fromBlock":"0x989680","toBlock":"0x9959d0","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",null,["0x0000000000000000000000001806c11be0f9b9af9e626a58904f3e5827b67be7","0x0000000000000000000000003c8fb6d064ceffc0f045f7b4aee6b3a4cefb4758"]]}], "id": "getblock.io"}
```

Response

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": []
}
```

## Body Parameters

Here is the list of body parameters for the `eth_getLogs` method response:

1. **jsonrpc**: A string specifying the version of the JSON-RPC protocol used. It is typically set to "2.0".
2. **id**: A unique identifier for the request. This is used to match the response with the request that it corresponds to. In this case, it is set to "getblock.io".
3. **result**: An array containing the logs that match the filter criteria specified in the request. Each log object in the array includes various details such as:
   * **address**: The contract address from which this log originated.
   * **topics**: An array of 32-byte log topics. Topics are indexed arguments of the log.
   * **data**: The non-indexed arguments of the log, encoded as a hexadecimal string.
   * **blockNumber**: The block number where this log was mined, encoded as a hexadecimal.
   * **transactionHash**: The hash of the transaction from which this log originated.
   * **transactionIndex**: The index position of the transaction within the block, encoded as a hexadecimal.
   * **blockHash**: The hash of the block where this log was mined.
   * **logIndex**: The index position of the log in the block, encoded as a hexadecimal.
   * **removed**: A boolean indicating whether the log was removed due to a chain reorganization. Logs can be removed if they are part of a block that is no longer in the canonical chain.

## Use Case

Here are some use-cases for the `eth_getLogs` method:

1. **Event Monitoring and Notification Systems:**\
   The `eth_getLogs` method is crucial for building applications that monitor smart contract events on the Ethereum blockchain. Developers can use this method to listen for specific events emitted by smart contracts, such as token transfers or contract updates. By setting up a notification system that triggers alerts when certain events occur, businesses and developers can stay informed about important changes or activities on the blockchain in real-time.
2. **Data Analysis and Reporting:**\
   Blockchain data is valuable for insights and analytics. The `eth_getLogs` method allows developers to retrieve logs of past blockchain events, which can be used for data analysis and reporting purposes. For instance, a financial analytics platform could use this method to gather historical transaction data for a specific token or contract, enabling users to perform trend analysis, assess market behaviors, or generate comprehensive reports on blockchain activities.
3. **Security and Compliance Auditing:**\
   In the realm of security and compliance, the `eth_getLogs` method is an essential tool for auditing blockchain activities. By fetching logs of specific events, auditors can verify that transactions and interactions with smart contracts comply with regulatory standards and internal policies. This method helps in creating a transparent and immutable record of blockchain activities, which is crucial for ensuring accountability and maintaining trust in decentralized applications.

## Code for eth\_getLogs

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {"jsonrpc": "2.0", "method": "eth_getLogs", "params": [{"address":["cc2e32f2388f0096fae9b055acffd76d4b3e5532","E518C608A37E2A262050E10BE0C9D03C7A0877F3"],"fromBlock":"0x989680","toBlock":"0x9959d0","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",null,["0x0000000000000000000000001806c11be0f9b9af9e626a58904f3e5827b67be7","0x0000000000000000000000003c8fb6d064ceffc0f045f7b4aee6b3a4cefb4758"]]}], "id": "getblock.io"}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)
```

## Common Errors

**Common Errors**\
When using the eth\_getLogs RPC Tron method, the following issues may occur:

* **Invalid Address Format**: The address provided is not in a valid hexadecimal format. Ensure that all addresses are correctly formatted with a '0x' prefix followed by 40 hexadecimal characters.
* **Block Range Too Large**: Specifying a block range that is too large can result in timeouts or excessive data retrieval. Consider narrowing the block range to improve performance and reduce the load on the node.
* **Missing or Incorrect Topics**: If topics are not specified correctly or are missing, the logs returned may not match the intended filters. Double-check the topic hashes and ensure they align with the event signatures you are targeting.
* **Node Synchronization Issues**: If the node is not fully synchronized with the network, it might return incomplete or outdated logs. Verify that your node is up-to-date and fully synchronized before querying logs.

Using the eth\_getLogs method in Web3 applications allows developers to efficiently filter and retrieve event logs from the blockchain, enabling real-time monitoring and analysis of contract events. This functionality is crucial for building responsive DApps that rely on event-driven architectures, providing users with timely and relevant information.

### conclusion

The `eth_getLogs` RPC method is a powerful tool for retrieving specific event logs from the Ethereum blockchain, based on parameters such as address, block range, and topics. This method is crucial for developers and analysts who need to track specific events or transactions efficiently. While Ethereum's `eth_getLogs` is widely used, it's important to note that similar functionalities are available in other blockchain networks like Tron, highlighting the versatility and adaptability of blockchain technology across different platforms.
