---
description: >-
  eth_getCode in JSON-RPC API Interface retrieves smart contract code from a specific address on the BSC network.
---

# eth_getCode

{% hint style="success" %}
The RPC method retrieves the bytecode of a smart contract deployed on the Binance Smart Chain, aiding in contract verification and analysis.&#x20;
{% endhint %}

The eth_getCode Web3 method is an essential function within the eth_getCode RPC protocol, designed to fetch the smart contract code stored at a given address on the Binance Smart Chain (BSC). By providing the target address and an optional block number, developers can access the bytecode of deployed contracts, enabling them to verify and interact with decentralized applications. This method is particularly useful for auditing and debugging smart contracts, as it ensures that the code executed on the blockchain matches the expected output. The eth_getCode Web3 method returns the contract's bytecode in hexadecimal format, which can be further analyzed or used in other blockchain operations. As part of the JSON-RPC API, it facilitates seamless integration into various blockchain development environments, enhancing the developer's ability to manage and interact with smart contracts on the BSC network effectively.

### Supported Networks

The eth_getCode REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_getCode method needs to be executed. Do not highlight the method name or enclose it in quotation marks.

- **Address**
  - **Type**: String
  - **Description**: The address of the smart contract for which the code is being requested.
  - **Required**: Yes
  - **Default/Supported Values**: Must be a valid Ethereum address in hexadecimal format, starting with "0x".

- **Block Parameter**
  - **Type**: String
  - **Description**: The block number, block hash, or one of the predefined block tags ("latest", "earliest", "pending") to specify the state of the blockchain from which to retrieve the code.
  - **Required**: Yes
  - **Default/Supported Values**: "latest", "earliest", "pending", or a specific block number/hash in hexadecimal format.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_getCode

#### Request

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

### Response


```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x"
}

```

### Body Parameters

Here is the list of body parameters for eth_getCode method:

1. **jsonrpc**: Specifies the version of the JSON-RPC protocol. For Ethereum, this is typically "2.0".

2. **id**: An identifier for the request, which can be any string or number. This is used to match the response with the request. In the example, it is "getblock.io".

3. **result**: The result of the method call, which is the bytecode of the contract at the given address. In the example, it is "0x", indicating no code is present at the address.

### Use Cases

Here are some use-cases for eth_getCode method:

1. **Smart Contract Verification**: Developers often use this method to verify the deployment of a smart contract on the Ethereum blockchain. By retrieving the bytecode associated with a specific address, developers can confirm that the contract has been correctly deployed and is functioning as expected.

2. **Security Audits**: Security auditors utilize this method to inspect and analyze the bytecode of smart contracts. By examining the code, auditors can identify potential vulnerabilities or malicious code that could compromise the security of the contract or its users.

3. **Contract Existence Check**: This method can also be used to determine whether an address is associated with a smart contract or a regular wallet. If the returned bytecode is empty, it indicates that the address is likely a regular user account, whereas non-empty bytecode suggests the presence of a smart contract. This is particularly useful for applications that need to differentiate between contract addresses and user addresses.

### Code for eth_getCode

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
  "result": "0x"
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
When using the eth_getCode JSON-RPC API BSC method, the following issues may occur:  
- Invalid address format: Ensure the address is a valid hexadecimal Ethereum address, prefixed with '0x'. Double-check for any typos or missing characters.  
- Block parameter issue: The block parameter must be set to 'latest' or a valid block number. Using an incorrect block identifier can result in an error or unexpected data.  
- Network connectivity problems: Ensure your connection to the BSC network is stable and properly configured. Network issues can lead to failed requests or timeouts.  
- Contract not deployed: If the specified address does not have any associated smart contract code, the method will return '0x'. Verify that the contract is deployed to the correct address on the BSC network.

The eth_getCode method is a powerful tool in Web3 applications, allowing developers to retrieve smart contract bytecode from the blockchain. This functionality is essential for verifying contract deployments, auditing code, and ensuring the integrity of decentralized applications.

### conclusion

The eth_getCode JSON-RPC method is used to retrieve the bytecode of a smart contract deployed at a specific address on the Ethereum blockchain, or compatible networks like BSC (Binance Smart Chain). By providing the contract address and a block identifier, such as "latest," users can access the most recent version of the contract's code. This functionality is crucial for developers and auditors who need to verify or interact with smart contracts on Ethereum and BSC.
