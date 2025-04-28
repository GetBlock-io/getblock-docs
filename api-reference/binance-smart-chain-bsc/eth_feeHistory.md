---
description: >-
  Access historical gas fee data using the eth_feeHistory method in the JSON-RPC
  API Interface for efficient transaction cost analysis on BSC.
---

# eth\_feeHistory - Binance Smart Chain

{% hint style="success" %}
The RPC method retrieves historical gas fee data on BSC, aiding users in estimating transaction costs and optimizing gas usage.
{% endhint %}

The `eth_feeHistory` method in the BSC protocol provides a snapshot of historical gas fee data, crucial for understanding recent network activity. Utilizing the `eth_feeHistory` Web3 interface, developers can retrieve information about gas prices over a range of recent blocks, aiding in more accurate fee estimations.

Through the `eth_feeHistory` RPC protocol, users can specify the number of blocks and the priority fee percentile to obtain detailed insights into gas price trends. This method is essential for optimizing transaction costs and enhancing the efficiency of applications interacting with the Binance Smart Chain.

### Supported Networks

The `eth_feeHistory` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_feeHistory` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Parameter 1: `blockCount`** (required)
  * **Type**: Integer
  * **Description**: The number of blocks requested in the fee history. This determines how many blocks of historical data will be returned.
  * **Supported Values**: Any positive integer value.
* **Parameter 2: `newestBlock`** (required)
  * **Type**: String or Integer
  * **Description**: The block number or the string "latest" to specify the most recent block for which to fetch the fee history.
  * **Supported Values**: "latest" or a specific block number in hexadecimal format.
* **Parameter 3: `rewardPercentiles`** (optional)
  * **Type**: Array of Numbers
  * **Description**: A list of percentile values that define the transaction fee percentiles to be returned for each block. These values should be between 0 and 100.
  * **Default/Supported Values**: Any array of numbers between 0 and 100, such as `[25, 75]`.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_feeHistory` :

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

**Response**

Below is a sample JSON response returned by `eth_feeHistory` upon a successful call:

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

Here is the list of body parameters for the `eth_feeHistory` method:

1. **oldestBlock**: This parameter represents the block number of the oldest block in the range being queried. In the response, it is given as `"0x2e64327"`.
2. **reward**: This parameter provides the list of rewards per block in the range. In the response, it is represented as a nested array with values such as `"0x3840"` and `"0x47868c00"`.
3. **baseFeePerGas**: This parameter indicates the base fee per gas unit for each block in the range. In the response, it is given as an array with values like `"0x0"`.
4. **gasUsedRatio**: This parameter shows the ratio of gas used to the gas limit for each block in the range. In the response, it is represented as an array with a value of `0.0846828`.
5. **baseFeePerBlobGas**: This parameter indicates the base fee per blob gas unit for each block in the range. In the response, it is given as an array with values like `"0x1"`.
6. **blobGasUsedRatio**: This parameter provides the ratio of blob gas used to the blob gas limit for each block in the range. In the response, it is represented as an array with a value of `0`.

### Use Cases

Here are some use-cases for `eth_feeHistory` method:

1. **Gas Price Estimation**: The `eth_feeHistory` method can be used to retrieve historical gas price data, which is crucial for estimating the appropriate gas price for transactions. By analyzing past block fees, developers can predict future gas prices and set optimal gas fees to ensure timely transaction confirmations without overpaying.
2. **Transaction Cost Analysis**: Developers and users can use `eth_feeHistory` to understand the cost trends of executing transactions on the Ethereum network over time. This information can be valuable for applications that need to budget for transaction costs or provide users with insights into how network congestion affects transaction fees.
3. **Network Congestion Monitoring**: By examining the historical fee data returned by `eth_feeHistory`, developers can monitor network congestion levels. This method provides insights into how busy the network has been and helps in making informed decisions about when to send transactions to minimize costs and delays.

### Code for eth\_feeHistory

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
  "method": "eth_feeHistory",
  "params": [4, "latest", [25, 75]],
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
  "method": "eth_feeHistory",
  "params": [4, "latest", [25, 75]],
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

When using the `eth_feeHistory` JSON-RPC API BSC method, the following issues may occur:

* Insufficient block range: Specifying a block range that exceeds the maximum allowed by the network can result in an error. To fix this, ensure the block range parameter is within the network's limits.
* Incorrect percentile values: Providing percentile values outside the 0-100 range can lead to unexpected results or errors. Verify that your percentile array contains valid values to prevent this issue.
* Network latency: High network latency can cause delayed responses or timeouts when fetching fee history data. Implement retry logic or increase the timeout settings to mitigate this problem.

Using the `eth_feeHistory` method in Web3 applications provides valuable insights into gas fee trends, enabling developers to optimize transaction costs. By analyzing historical fee data, applications can offer users more accurate fee estimations, enhancing the overall user experience and transaction efficiency.

### Conclusion

The `eth_feeHistory` JSON-RPC method provides valuable insights into Ethereum's gas fee trends by returning historical gas data, which can be crucial for optimizing transaction costs. While primarily used on Ethereum, similar methods can be adapted for other networks like BSC to enhance transaction efficiency. Understanding `eth_feeHistory` can significantly aid in navigating the complex landscape of blockchain fees.
