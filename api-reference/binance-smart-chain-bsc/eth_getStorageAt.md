---
description: >-
  Access storage data at a specific address using eth_getStorageAt via the JSON-RPC API Interface in the BSC protocol. Technical and user-friendly.
---

# eth_getStorageAt

{% hint style="success" %}
The RPC method retrieves the value stored at a specific storage position of a smart contract on Binance Smart Chain.&#x20;
{% endhint %}

The `eth_getStorageAt` method in the BSC protocol allows users to retrieve the value stored at a specific storage position of a contract. Using the `eth_getStorageAt Web3` interface, developers can easily interact with smart contract storage by providing the contract address, storage position, and block number.

As part of the `eth_getStorageAt RPC protocol`, this method facilitates direct access to the underlying data of smart contracts, enabling precise data retrieval for debugging or analysis. Its efficient design ensures that developers can seamlessly integrate storage queries into their applications, enhancing the utility and flexibility of decentralized applications.

## Supported Networks

The eth_getStorageAt JSON-RPC API method supports the following network types:
- **Mainnet**
- **Testnet**

## Parameters

Here is the list of parameters `eth_getStorageAt` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

- **Address** (required)
  - **Type**: String
  - **Description**: The address of the smart contract from which to read the storage.
  - **Example**: `"0xE592427A0AEce92De3Edee1F18E0157C05861564"`

- **Position** (required)
  - **Type**: String
  - **Description**: The position in storage to read from. It is given as a hex string.
  - **Example**: `"0x0"`

- **Block Parameter** (required)
  - **Type**: String
  - **Description**: The block number or one of the predefined block parameters like `"latest"`, `"earliest"`, or `"pending"`.
  - **Supported Values**: `"latest"`, `"earliest"`, `"pending"`, or a specific block number in hex format.
  - **Example**: `"latest"`

These parameters are part of the request body in JSON-RPC format.

# Request Example

##### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```


#### Request

Here’s a sample cURL request using eth_getStorageAt :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getStorageAt",
  "params": ["0xE592427A0AEce92De3Edee1F18E0157C05861564", "0x0", "latest"],
  "id": 1
}
```
{% endtab %}
{% endtabs %}

#### Response

Below is a sample JSON response returned by eth_getStorageAt upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x0000000000000000000000000000000000000000000000000000000000000000"
}

```

## Body Parameters

Here is the list of body parameters for `eth_getStorageAt` method:

1. **`jsonrpc`**: The version of the JSON-RPC protocol, typically "2.0".

2. **`id`**: A unique identifier for the request, which helps in matching responses to requests.

3. **`result`**: The storage value at the specified position, returned as a hexadecimal string. In this example, it is "0x0000000000000000000000000000000000000000000000000000000000000000".

## Use Cases

Here are some use-cases for `eth_getStorageAt` method:

1. **Smart Contract State Inspection**: Developers can use `eth_getStorageAt` to directly inspect the storage of a smart contract at a specific slot. This is particularly useful for debugging and verifying that the contract's state aligns with expectations. For example, if a contract uses a mapping or an array for storage, developers can query specific slots to check the values stored at those locations.

2. **Security Audits and Vulnerability Assessments**: Security auditors can leverage `eth_getStorageAt` to analyze the storage layout of a contract and identify potential vulnerabilities. By examining the data stored at various slots, auditors can ensure that sensitive information is not inadvertently exposed and that the contract's state cannot be manipulated in unintended ways.

3. **Data Recovery and Forensics**: In cases where a contract's state has been altered unexpectedly or data has been lost due to a bug, `eth_getStorageAt` can be used to retrieve historical storage data. This can help developers understand how the state has changed over time and assist in recovering or reconstructing the lost data.

## Code for eth_getStorageAt

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
  "result": "0x0000000000000000000000000000000000000000000000000000000000000000"
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
  "result": "0x0000000000000000000000000000000000000000000000000000000000000000"
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

When using the `eth_getStorageAt` JSON-RPC API BSC method, the following issues may occur:
- **Incorrect Address Format**: If the contract address is not in the correct hexadecimal format with a '0x' prefix, the request will fail. Ensure the address is properly formatted and valid.
- **Invalid Storage Slot Index**: Providing an invalid or out-of-range storage slot index can lead to unexpected results or errors. Verify that the storage slot index is correct and within the contract's storage bounds.
- **Network Sync Issues**: If the BSC node is not fully synchronized with the network, the data retrieved may be outdated or incorrect. Use a fully synchronized node or a reliable node service provider for accurate results.

Using the `eth_getStorageAt` method in Web3 applications allows developers to directly access and interact with the storage variables of smart contracts. This capability is essential for reading contract states and implementing features that rely on real-time or historical data stored on the blockchain, enhancing the functionality and responsiveness of decentralized applications.

## Conclusion

The JSON-RPC method `eth_getStorageAt` is used to retrieve the value stored at a specific storage position of a smart contract on the Ethereum blockchain. In this request, the method is querying the storage at position `0x0` for the contract at address `0xE592427A0AEce92De3Edee1F18E0157C05861564` on the latest block. This method is crucial for developers and auditors who need to inspect or verify contract state on Ethereum or BSC, as it provides direct access to the underlying storage data.
