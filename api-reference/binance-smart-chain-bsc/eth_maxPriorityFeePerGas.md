---
description: >-
  Retrieve the current max priority fee per gas on BSC using
  eth_maxPriorityFeePerGas via the JSON-RPC API Interface.
---

# eth\_maxPriorityFeePerGas - Binance Smart Chain

{% hint style="success" %}
The method estimates the current optimal priority fee per gas for transactions on the Binance Smart Chain, enhancing transaction speed and efficiency.
{% endhint %}

The `eth_maxPriorityFeePerGas` method in the BSC protocol is a JSON-RPC API call that retrieves the maximum priority fee per gas unit suggested for transactions. This method aids users in determining an appropriate tip to include in their transactions, ensuring timely processing by miners on the network.

Utilizing the `eth_maxPriorityFeePerGas` Web3 interface, developers can integrate this functionality into their applications to enhance user experience by providing dynamic fee calculations. The `eth_maxPriorityFeePerGas` RPC protocol ensures efficient communication between clients and the BSC network, optimizing transaction handling.

### Supported Networks

The eth\_maxPriorityFeePerGas JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

None: This method does not require any parameters.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using eth\_maxPriorityFeePerGas :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_maxPriorityFeePerGas",
  "params": [],
  "id": 1
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by eth\_maxPriorityFeePerGas upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x3b9aca00"
}

```

### Body Parameters

Here is the list of body parameters for the `eth_maxPriorityFeePerGas` method:

1. **jsonrpc**: This parameter specifies the version of the JSON-RPC protocol being used. In this case, it is "2.0".
2. **id**: This is an identifier for the request. It is used to match the response with the request. In this example, the id is 1.
3. **result**: This parameter contains the result of the `eth_maxPriorityFeePerGas` method call. It is the maximum priority fee per gas that a user is willing to pay to have their transaction included in a block, represented in hexadecimal format. In this response, the result is "0x3b9aca00".

### Use Cases

Here are some use-cases for `eth_maxPriorityFeePerGas` method:

1. **Transaction Cost Estimation**: When developing decentralized applications (dApps) on Ethereum, it's crucial to estimate transaction costs accurately. By using the `eth_maxPriorityFeePerGas` method, developers can retrieve the current maximum priority fee that users are paying to get their transactions processed quickly. This information helps in setting an appropriate `maxPriorityFeePerGas` for transactions, ensuring they are mined promptly without overpaying.
2. **Dynamic Fee Adjustment**: In volatile network conditions, the gas fees can fluctuate significantly. The `eth_maxPriorityFeePerGas` method provides real-time data that developers can use to adjust transaction fees dynamically. This is particularly useful for wallets or services that aim to optimize transaction costs for users by automatically adjusting fees based on current network conditions.
3. **User Experience Enhancement**: For applications that aim to provide a seamless user experience, understanding the current fee market is essential. By integrating the `eth_maxPriorityFeePerGas` method, developers can offer users recommendations or presets for transaction fees, helping them choose the right balance between cost and speed. This can lead to higher user satisfaction as transactions are confirmed in a timely manner without unnecessary expense.

### Code for eth\_maxPriorityFeePerGas

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
  "result": "0x3b9aca00"
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
  "result": "0x3b9aca00"
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

When using the `eth_maxPriorityFeePerGas` JSON-RPC API BSC method, the following issues may occur:

* **Network Congestion:** If the network is experiencing high traffic, the returned priority fee might be higher than expected. To address this, monitor network conditions and adjust your transaction strategy accordingly.
* **Outdated Client Software:** Using an outdated client version might lead to unexpected errors or inaccurate fee estimates. Ensure that your client software is up-to-date to maintain compatibility with the latest protocol changes.
* **Rate Limiting:** Excessive requests to the BSC node can result in rate limiting, leading to delayed or failed responses. Implement request throttling and caching strategies to optimize your application's performance.
* **Node Synchronization Issues:** If the BSC node is not fully synchronized with the network, it might provide incorrect fee estimates. Verify node synchronization status and consider using a reliable node provider to mitigate this risk.

Using the `eth_maxPriorityFeePerGas` method in Web3 applications allows developers to dynamically adjust transaction fees based on current network conditions, ensuring timely transaction processing. This capability enhances user experience by reducing transaction delays and optimizing gas costs, making it a valuable tool for efficient transaction management in decentralized applications.

### Conclusion

The `eth_maxPriorityFeePerGas` method in the JSON-RPC API is crucial for determining the maximum priority fee per gas on networks like Ethereum and BSC. By leveraging `eth_maxPriorityFeePerGas`, developers can optimize transaction costs efficiently. This method is essential for ensuring transactions are processed promptly without overpaying.
