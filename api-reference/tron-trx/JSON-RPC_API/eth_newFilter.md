---
description: >-
  Discover 'eth_newFilter' in the Tron JSON-RPC API Interface for efficient
  event filtering.
---

# eth\_newFilter - TRON

## Description

The 'eth\_newFilter' method in the Tron protocol's Web3 integration allows developers to create new filters for efficiently monitoring blockchain events. Through the 'eth\_newFilter' RPC protocol, users can specify filter criteria such as address, topics, and block range to capture specific events or transactions on the Tron network. This method supports decentralized application developers in managing and responding to blockchain activity with precision. By leveraging the 'eth\_newFilter' Web3 functionality, developers can streamline event handling, ensuring that applications remain responsive and up-to-date with the latest blockchain changes. This tool is essential for any developer looking to harness the full potential of Tron’s blockchain capabilities.

## Supported Networks

The eth\_newFilter RPC method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters eth\_newFilter method needs to be executed.

* **address** (Optional)
  * **Type**: Array of strings
  * **Description**: Specifies the contract addresses from which logs should be captured. If not specified, logs are collected from all addresses.
  * **Supported Values**: Ethereum addresses in hexadecimal format.
  * **Example**: `["cc2e32f2388f0096fae9b055acffd76d4b3e5532","E518C608A37E2A262050E10BE0C9D03C7A0877F3"]`
* **fromBlock** (Optional)
  * **Type**: String
  * **Description**: The block number from which to start looking for logs. If not specified, the default is the latest block.
  * **Supported Values**: Block number in hexadecimal format (e.g., `"0x989680"`).
* **toBlock** (Optional)
  * **Type**: String
  * **Description**: The block number up to which to look for logs. If not specified, the default is the latest block.
  * **Supported Values**: Block number in hexadecimal format (e.g., `"0x9959d0"`).
* **topics** (Optional)
  * **Type**: Array of strings or arrays
  * **Description**: Allows filtering of logs by topics. Each topic can also be an array of multiple topics to match any of them.
  * **Supported Values**: Topics in hexadecimal format. `null` can be used to match any topic.
  * **Example**: `["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef", null, ["0x0000000000000000000000001806c11be0f9b9af9e626a58904f3e5827b67be7", "0x0000000000000000000000003c8fb6d064ceffc0f045f7b4aee6b3a4cefb4758"]]`

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using eth\_newFilter

Request

```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' 
--header 'Content-Type: application/json' 
{"jsonrpc": "2.0", "method": "eth_newFilter", "params": [{"address":["cc2e32f2388f0096fae9b055acffd76d4b3e5532","E518C608A37E2A262050E10BE0C9D03C7A0877F3"],"fromBlock":"0x989680","toBlock":"0x9959d0","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",null,["0x0000000000000000000000001806c11be0f9b9af9e626a58904f3e5827b67be7","0x0000000000000000000000003c8fb6d064ceffc0f045f7b4aee6b3a4cefb4758"]]}], "id": "getblock.io"}
```

Response

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x1b765dc7b3787a1a7fa4db3d8c875231"
}
```

## Body Parameters

Here is the list of body parameters for the `eth_newFilter` method:

1. **jsonrpc**: This specifies the version of the JSON-RPC protocol. In this case, it is "2.0".
2. **id**: A unique identifier for the request. In this example, it is "getblock.io".
3. **result**: The response returned by the method, which is a filter identifier. In this example, it is "0x1b765dc7b3787a1a7fa4db3d8c875231". This filter ID can be used in subsequent requests to interact with the filter.

## Use Case

Here are some use-cases for the `eth_newFilter` method in Web3 programming:

1. **Monitoring Token Transfers**: The `eth_newFilter` method can be used to monitor specific token transfers on the Ethereum blockchain. By specifying the `topics` parameter with the transfer event signature (`0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef`), developers can filter for ERC-20 token transfers. In the provided JSON-RPC call, the filter is set to track transfers involving specific addresses, allowing dApps to react to incoming or outgoing token transactions in real-time.
2. **Tracking Contract Events**: Smart contracts often emit events to signal specific occurrences within the contract's lifecycle. The `eth_newFilter` method allows developers to listen for these events by specifying the contract address and event topics. This is particularly useful for decentralized applications (dApps) that need to update their state or notify users when certain conditions are met or actions are taken within a smart contract.
3. **Historical Data Analysis**: By setting the `fromBlock` and `toBlock` parameters, developers can use the `eth_newFilter` method to query historical blockchain data for specific events or transactions. This can be useful for analyzing past activity, auditing smart contract interactions, or gathering insights into user behavior and transaction patterns over a specified period.

These use cases demonstrate the versatility of the `eth_newFilter` method in enabling dynamic and responsive interactions with the Ethereum blockchain, making it an essential tool for developers building decentralized applications.

## Code for eth\_newFilter

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {"jsonrpc": "2.0", "method": "eth_newFilter", "params": [{"address":["cc2e32f2388f0096fae9b055acffd76d4b3e5532","E518C608A37E2A262050E10BE0C9D03C7A0877F3"],"fromBlock":"0x989680","toBlock":"0x9959d0","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",null,["0x0000000000000000000000001806c11be0f9b9af9e626a58904f3e5827b67be7","0x0000000000000000000000003c8fb6d064ceffc0f045f7b4aee6b3a4cefb4758"]]}], "id": "getblock.io"}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)
```

## Common Errors

Common Errors\
When using the eth\_newFilter RPC Tron method, the following issues may occur:

* Invalid Address Format: Ensure that all addresses are in the correct hexadecimal format, starting with '0x'. Double-check the case sensitivity as Tron addresses are case-sensitive.
* Incorrect Block Range: The 'fromBlock' and 'toBlock' parameters should be valid block numbers within the blockchain's current range. Verify that these values are up-to-date and correctly formatted in hexadecimal.
* Malformed Topics: Ensure that the topics array is properly structured, with each topic being a 32-byte hexadecimal string. Review the event signature and indexed parameters to confirm correctness.
* Network Connectivity Issues: A failure to connect to the Tron network could result from network configurations or endpoint issues. Ensure that your network settings are correctly configured and that the endpoint is accessible.

The eth\_newFilter method is a powerful tool in Web3 applications, allowing developers to efficiently monitor blockchain events and changes. By setting up filters, you can track specific transactions or events, enabling responsive and dynamic app features that enhance user interaction and data handling.

### conclusion

The `eth_newFilter` RPC method is a powerful tool in Ethereum, allowing developers to create a filter object to monitor specific events or transactions within a specified block range. By specifying addresses, block ranges, and topics, developers can efficiently track and respond to blockchain activities. While Ethereum's `eth_newFilter` provides a robust event filtering mechanism, it's interesting to contrast it with platforms like Tron, which offer different approaches to smart contract interaction and event handling. This comparison highlights the versatility and adaptability of blockchain technologies in catering to diverse developer needs.
