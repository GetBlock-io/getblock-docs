# eth_getFilterLogs


## Meta Description
The 'eth_getFilterLogs' method in Tron’s JSON-RPC API Interface retrieves filtered event logs efficiently.

## Description
The 'eth_getFilterLogs' Web3 method is an essential component of the Tron protocol's RPC protocol, designed to retrieve log entries that match a specified filter. This method allows developers to efficiently query and access event logs that have been filtered based on specific criteria such as block range, contract address, or event signature. By leveraging the 'eth_getFilterLogs' RPC protocol, users can streamline the process of monitoring and analyzing blockchain events, making it an invaluable tool for developers working on decentralized applications. Its integration into the JSON-RPC API Interface ensures seamless interaction with the Tron network, providing a reliable and consistent method for accessing blockchain data.

## Supported Networks
The eth_getFilterLogs RPC method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters eth_getFilterLogs method needs to be executed.

- **Filter ID (required)**
  - **Type:** String
  - **Description:** The filter ID is a unique identifier for the filter created previously using `eth_newFilter` or similar methods. It is used to fetch the logs that match the filter criteria.
  - **Default/Supported Values:** A valid filter ID, which is typically a hexadecimal string starting with "0x", such as "0xc11a84d5e906ecb9f5c1eb65ee940b154ad37dce8f5ac29c80764508b901d996".

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_getFilterLogs

Request
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' 
--header 'Content-Type: application/json' 
{"jsonrpc": "2.0", "method": "eth_getFilterLogs", "params": ["0xc11a84d5e906ecb9f5c1eb65ee940b154ad37dce8f5ac29c80764508b901d996"], "id": "getblock.io"}
```

Response
```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "error": {
    "code": -32000,
    "message": "filter not found",
    "data": "{}
```
## Body Parameters

Here is the list of body parameters for the `eth_getFilterLogs` method:

1. **id**: A unique identifier for the request. It is used to match the response with the request. In the example provided, the id is `"getblock.io"`.

2. **jsonrpc**: The version of the JSON-RPC protocol. For Ethereum, this is typically `"2.0"`.

3. **result**: An array of log objects, each containing details about a specific log entry. Each log object includes the following fields:
   - **address**: The address of the contract that generated the log.
   - **topics**: An array of 32-byte log topics. These are indexed parameters from the event.
   - **data**: The non-indexed arguments of the log, stored as a hexadecimal string.
   - **blockNumber**: The block number where the log was generated, represented as a hexadecimal string.
   - **transactionHash**: The hash of the transaction that generated the log.
   - **transactionIndex**: The index of the transaction within the block, represented as a hexadecimal string.
   - **blockHash**: The hash of the block where the log was generated.
   - **logIndex**: The index position of the log in the block, represented as a hexadecimal string.
   - **removed**: A boolean indicating whether the log was removed due to a chain reorganization.

4. **error**: If an error occurs, this object will be present and contain:
   - **code**: A numeric error code.
   - **message**: A human-readable error message.
   - **data**: Additional error information, if available.

Note: The `eth_getFilterLogs` method is used to retrieve logs that match a filter, which is created using methods like `eth_newFilter`. The error message "filter not found" indicates that the specified filter ID does not exist or has expired.

## Use Case

Here are some use-cases for the `eth_getFilterLogs` method in Web3 programming:

1. **Event Monitoring and Notification**: One of the primary use cases for the `eth_getFilterLogs` method is to monitor specific events on the Ethereum blockchain. Developers can create filters to listen for particular smart contract events, such as transfers, approvals, or custom events emitted by decentralized applications (dApps). By retrieving logs that match these filters, dApps can trigger notifications or execute specific functions in response to these events, enabling real-time interactivity and automation based on blockchain activity.

2. **Historical Data Retrieval**: Another use case involves retrieving historical logs from the blockchain for analytical purposes. Developers can use `eth_getFilterLogs` to access past events that match specific criteria, allowing them to analyze transaction patterns, audit contract interactions, or gather data for reporting and compliance purposes. This is particularly useful for applications that require a comprehensive view of blockchain activity over time, such as financial services or supply chain tracking.

3. **Debugging and Testing**: During the development and testing phases of smart contracts, developers can utilize `eth_getFilterLogs` to debug and verify the behavior of their contracts. By reviewing the logs generated during contract execution, developers can ensure that events are being emitted correctly and that the contract logic is functioning as intended. This method helps identify issues early in the development process, reducing the likelihood of errors in production environments.

## Code for eth_getFilterLogs


```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {"jsonrpc": "2.0", "method": "eth_getFilterLogs", "params": ["0xc11a84d5e906ecb9f5c1eb65ee940b154ad37dce8f5ac29c80764508b901d996"], "id": "getblock.io"}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)
```
## Common Errors

Common Errors  
When using the eth_getFilterLogs RPC Tron method, the following issues may occur:  
- Invalid Filter ID: If the filter ID provided is incorrect or expired, the method will not return the expected logs. Ensure that the filter ID is valid and has been recently created.  
- Network Mismatch: Attempting to retrieve logs from a network different from the one where the filter was created can result in errors. Double-check that your network settings match the intended environment.  
- Node Synchronization: If the connected node is not fully synchronized with the blockchain, it may not return accurate log data. Verify that your node is fully synced before making requests.  
- Permission Denied: Insufficient permissions on the node can prevent access to logs. Ensure that your client has the necessary permissions to execute this method.  

Using the eth_getFilterLogs method in Web3 applications allows developers to efficiently retrieve event logs based on specific filters, enhancing the ability to monitor and react to blockchain events in real time. This functionality is crucial for building responsive and data-driven decentralized applications, as it enables seamless integration with smart contract activities.

### conclusion

The `eth_getFilterLogs` RPC method is an essential tool for retrieving logs that match a specified filter in Ethereum, facilitating efficient event tracking and analysis. While Ethereum primarily uses this method, similar functionalities are also found in other blockchain platforms like Tron, showcasing the universal need for robust log retrieval mechanisms. This cross-platform utility underscores the importance of `eth_getFilterLogs` in maintaining transparency and operational insights within decentralized ecosystems.
