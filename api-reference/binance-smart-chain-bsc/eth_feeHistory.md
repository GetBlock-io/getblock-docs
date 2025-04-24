---
description: >-
  Retrieve historical gas fee data using eth_feeHistory in the JSON-RPC API Interface for better transaction cost estimation.
---

# eth_feeHistory

{% hint style="success" %}
The RPC method provides historical base fee, gas used, and reward data for recent blocks, aiding in gas price estimation on the Binance Smart Chain.&#x20;
{% endhint %}

The eth_feeHistory Web3 method is an essential tool in the Binance Smart Chain (BSC) protocol, designed to provide users with historical gas fee data. By leveraging the eth_feeHistory RPC protocol, developers and users can access a range of block-specific gas fee information, including base fee per gas and gas used ratios. This method aids in estimating transaction costs by offering a detailed view of past fee trends, enabling more informed decisions for future transactions. The eth_feeHistory method accepts parameters such as the number of blocks, the highest block number, and a reward percentile array, returning a structured response that includes base fee per gas and gas used ratio for each block. This functionality is crucial for optimizing transaction costs and improving the efficiency of smart contract interactions within the BSC ecosystem.

### Supported Networks

The eth_feeHistory REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_feeHistory method needs to be executed:

- **blockCount** (required)
  - **Type**: Integer
  - **Description**: The number of blocks in the requested range.
  - **Supported Values**: Any positive integer representing the number of blocks.

- **newestBlock** (required)
  - **Type**: String or Integer
  - **Description**: The highest block number in the range, or the string "latest" to specify the latest block.
  - **Supported Values**: A hexadecimal string representing a block number or the string "latest".

- **rewardPercentiles** (optional)
  - **Type**: Array of Numbers
  - **Description**: A list of percentile values to sample from each block's gas used.
  - **Supported Values**: A list of numbers between 0 and 100, representing percentiles.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_feeHistory

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_feeHistory",
  "params": [4, "latest", [25, 75]],
  "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

### Response


```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "oldestBlock": "0x2e64327",
    "reward": [
      [
        "0x3840",
        "0x47868c00"
      ]
    ],
    "baseFeePerGas": [
      "0x0",
      "0x0"
    ],
    "gasUsedRatio": [
      0.0846828
    ],
    "baseFeePerBlobGas": [
      "0x1",
      "0x1"
    ],
    "blobGasUsedRatio": [
      0
    ]
  }
}

```

### Body Parameters

Here is the list of body parameters for eth_feeHistory method:

1. `oldestBlock`: The oldest block in the range, represented as a hexadecimal string.
2. `reward`: An array containing arrays of hexadecimal strings, representing the block rewards.
3. `baseFeePerGas`: An array of hexadecimal strings, indicating the base fee per gas unit for the blocks.
4. `gasUsedRatio`: An array of floating-point numbers, representing the ratio of gas used in each block.
5. `baseFeePerBlobGas`: An array of hexadecimal strings, indicating the base fee per blob gas unit.
6. `blobGasUsedRatio`: An array of numbers, representing the ratio of blob gas used.

### Use Cases

Here are some use-cases for eth_feeHistory method:

1. **Gas Price Estimation**: Developers can use this method to retrieve historical base fee per gas and priority fee data, which can help in estimating the optimal gas price for transactions. By analyzing recent blocks, applications can suggest competitive gas fees to ensure timely transaction confirmations while avoiding overpayment.

2. **Network Congestion Analysis**: By accessing historical fee data, developers and analysts can gain insights into network congestion trends over time. This information can be useful for identifying peak usage periods and understanding how network demand fluctuates, allowing for better planning and optimization of transaction scheduling.

3. **Dynamic Fee Adjustment**: Wallets and decentralized applications can leverage historical fee data to implement dynamic fee adjustment strategies. By understanding past fee patterns, these applications can automatically adjust transaction fees in real-time to optimize costs and improve the likelihood of successful and timely transaction processing.

### Code for eth_feeHistory

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
  "result": {
    "oldestBlock": "0x2e64327",
    "reward": [
      [
        "0x3840",
        "0x47868c00"
      ]
    ],
    "baseFeePerGas": [
      "0x0",
      "0x0"
    ],
    "gasUsedRatio": [
      0.0846828
    ],
    "baseFeePerBlobGas": [
      "0x1",
      "0x1"
    ],
    "blobGasUsedRatio": [
      0
    ]
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
When using the eth_feeHistory JSON-RPC API BSC method, the following issues may occur:  
- Incorrect Block Parameter: Specifying an invalid block parameter, such as a block number that exceeds the current blockchain height, can lead to errors. Ensure that the block parameter is within the valid range and formatted correctly.  
- Invalid Percentile Values: Using percentile values outside the accepted range of 0 to 100 can result in errors. Double-check that your percentile values are within this range to avoid issues.  
- Network Latency: High network latency can cause timeouts when fetching fee history data. Consider implementing retry logic in your application to handle such scenarios gracefully.  
- Unsupported Parameters: Passing unsupported or additional parameters not recognized by the BSC method can lead to unexpected errors. Review the method documentation to ensure all parameters are valid.

Using the eth_feeHistory method in Web3 applications offers the advantage of accessing historical gas fee data, which can be critical for optimizing transaction costs. By understanding past fee trends, developers can make more informed decisions about transaction timing and pricing, ultimately enhancing the user experience in decentralized applications.

### conclusion

The eth_feeHistory JSON-RPC method is a valuable tool for retrieving historical gas fee data on the Ethereum network. By specifying parameters such as block count and percentile, users can gain insights into past transaction costs. This functionality is crucial for developers and analysts working with Ethereum or similar networks like BSC, as it aids in optimizing transaction strategies and understanding network congestion trends.
