---
description: >-
  Retrieve smart contract bytecode using eth_getCode in the JSON-RPC API
  Interface for BSC, enabling efficient blockchain data access.
---

# eth\_getCode - BNB Smart Chain

{% hint style="success" %}
The RPC method retrieves the bytecode of a smart contract at a specific address on the BNB Smart Chain, verifying contract deployment.
{% endhint %}

The `eth_getCode` method in the BSC protocol is a crucial part of the `eth_getCode` Web3 interface, allowing users to retrieve the EVM bytecode of a smart contract at a specified address. By using the `eth_getCode` RPC protocol, developers can programmatically access contract code, facilitating contract analysis and debugging.

To call `eth_getCode`, provide the contract's address and the desired block number or use "latest" for the most recent block. The method returns the bytecode as a hexadecimal string. This functionality is essential for developers working with decentralized applications on the BNB Smart Chain, offering insights into contract behavior and state.

### Supported Networks

The `eth_getCode` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_getCode` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Address (required)**
  * **Type:** String
  * **Description:** The address of the contract to retrieve the code from.
  * **Supported Values:** A valid Ethereum address prefixed with "0x".
* **Block Parameter (required)**
  * **Type:** String
  * **Description:** The block number, or one of the string tags "latest", "earliest", or "pending", to specify the block context for the code retrieval.
  * **Supported Values:** "latest", "earliest", "pending", or a specific block number in hexadecimal format prefixed with "0x".

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_getCode` :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getCode",
  "params": ["0x5B56438000bAc5ed2c6E0c1EcFF4354aBfFaf889", "latest"],
  "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by `eth_getCode` upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x"
}

```

### Body Parameters

Here is the list of body parameters for `eth_getCode` method:

1. **`jsonrpc`**: This parameter specifies the version of the JSON-RPC protocol being used. For Ethereum, it is typically `"2.0"`.
2. **`id`**: This is an identifier for the request. It can be any string or number that the client uses to match the response with the request. In the example, it is `"getblock.io"`.
3. **`result`**: This parameter contains the output of the `eth_getCode` method. It is a string representing the bytecode of the smart contract at the specified address. In the example, it is `"0x"`, which indicates that there is no code at the specified address.

### Use Cases

Here are some use-cases for the `eth_getCode` method:

1. **Smart Contract Verification**: One of the primary uses of the `eth_getCode` method is to verify whether a specific address on the Ethereum blockchain is a smart contract. By retrieving the bytecode associated with an address, developers can determine if an address is a smart contract or a regular externally owned account (EOA). If the returned bytecode is not empty, it indicates the presence of a smart contract at that address.
2. **Contract Analysis and Security Audits**: Developers and security analysts can use `eth_getCode` to fetch the bytecode of a deployed smart contract for analysis and auditing purposes. This allows them to inspect the contract's code for vulnerabilities or to ensure that the deployed code matches the expected source code, helping maintain the security and integrity of decentralized applications.
3. **Interoperability and Integration**: When building applications that interact with existing smart contracts, developers can use `eth_getCode` to dynamically retrieve and verify the bytecode of contracts they intend to interact with. This ensures that the contract's code is as expected, which is particularly useful when integrating with third-party contracts or when building tools that need to support multiple versions of a contract.

### Code for eth\_getCode

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
  "method": "eth_getCode",
  "params": ["0x5B56438000bAc5ed2c6E0c1EcFF4354aBfFaf889", "latest"],
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
  "method": "eth_getCode",
  "params": ["0x5B56438000bAc5ed2c6E0c1EcFF4354aBfFaf889", "latest"],
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

When using the `eth_getCode` JSON-RPC API BSC method, the following issues may occur:

* Incorrect contract address: If the provided contract address is incorrect or not formatted properly, the method will return an empty response. Ensure that the address is a valid checksummed BSC address.
* Network synchronization delay: If the BSC node is not fully synchronized, it might return outdated or incorrect code. Verify that your node is fully synced with the latest block before making the request.
* Permission issues: Some nodes may restrict access to certain methods for security reasons. Confirm that your node configuration allows access to the `eth_getCode` method.
* Invalid block parameter: If the block parameter is not set to a valid block identifier like "latest" or a specific block number, the method might fail. Double-check that the block parameter is set correctly.

Using the `eth_getCode` method in Web3 applications provides the ability to retrieve the bytecode of a smart contract deployed on the BSC network. This is crucial for verifying contract deployment, auditing code, and ensuring that the contract's functionality matches expectations. By leveraging this method, developers can enhance the transparency and reliability of their decentralized applications.

### Conclusion

The `eth_getCode` JSON-RPC method is a crucial tool for retrieving the smart contract code stored at a specific address on blockchain networks like Ethereum and BSC. By using `eth_getCode`, developers can verify and interact with smart contracts, ensuring transparency and functionality within decentralized applications.
