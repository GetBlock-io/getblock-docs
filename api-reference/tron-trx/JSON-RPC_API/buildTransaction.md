---
description: >-
  Explore 'buildTransaction' in Tron’s JSON RPC API Interface for seamless
  transaction creation.
---

# buildTransaction - TRON

## Description

The 'buildTransaction' method in Tron’s Web3 framework is a crucial component of the buildTransaction RPC protocol, enabling developers to efficiently create transactions within the Tron blockchain. This method constructs a transaction object that includes essential details such as sender and receiver addresses, amount, and any additional parameters required for execution on the network. By leveraging the JSON-RPC API Interface, developers can seamlessly interact with the Tron network, ensuring secure and accurate transaction processing. The 'buildTransaction' method is designed to be user-friendly yet robust, catering to both novice and experienced developers seeking to integrate Tron blockchain capabilities into their applications. Whether you are building decentralized applications or exploring blockchain technology, 'buildTransaction' offers a reliable gateway to the Tron network’s transaction functionalities.

## Supported Networks

The buildTransaction RPC method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters the `buildTransaction` method needs to be executed.

| Param Name | Data Type      | Description                                 |
| ---------- | -------------- | ------------------------------------------- |
| from       | DATA, 20 Bytes | The address the transaction is sent from.   |
| to         | DATA, 20 Bytes | The address the transaction is directed to. |
| value      | DATA           | The transfer amount.                        |

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using buildTransaction

Request

```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' 
--header 'Content-Type: application/json' 
{"jsonrpc": "2.0",
"method": "buildTransaction",
"params": ["0xC4DB2C9DFBCB6AA344793F1DDA7BD656598A06D8", "transferTokenContract", "0x245498", "[{\"constant\":false,\"inputs\":[],\"name\":\"getResultInCon\",\"outputs\":[{\"name\":\"\",\"type\":\"trcToken\"},{\"name\":\"\",\"type\":\"uint256\"},{\"name\":\"\",\"type\":\"uint256\"}],\"payable\":true,\"stateMutability\":\"payable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"name\":\"toAddress\",\"type\":\"address\"},{\"name\":\"id\",\"type\":\"trcToken\"},{\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"TransferTokenTo\",\"outputs\":[],\"payable\":true,\"stateMutability\":\"payable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[],\"name\":\"msgTokenValueAndTokenIdTest\",\"outputs\":[{\"name\":\"\",\"type\":\"trcToken\"},{\"name\":\"\",\"type\":\"uint256\"},{\"name\":\"\",\"type\":\"uint256\"}],\"payable\":true,\"stateMutability\":\"payable\",\"type\":\"function\"},{\"inputs\":[],\"payable\":true,\"stateMutability\":\"payable\",\"type\":\"constructor\"}]\n", "6080604052d3600055d2600155346002556101418061001f6000396000f3006080604052600436106100565763ffffffff7c010000000000000000000000000000000000000000000000000000000060003504166305c24200811461005b5780633be9ece71461008157806371dc08ce146100aa575b600080fd5b6100636100b2565b60408051938452602084019290925282820152519081900360600190f35b6100a873ffffffffffffffffffffffffffffffffffffffff600435166024356044356100c0565b005b61006361010d565b600054600154600254909192565b60405173ffffffffffffffffffffffffffffffffffffffff84169082156108fc029083908590600081818185878a8ad0945050505050158015610107573d6000803e3d6000fd5b50505050565bd3d2349091925600a165627a7a72305820a2fb39541e90eda9a2f5f9e7905ef98e66e60dd4b38e00b05de418da3154e7570029", 100, 11111111111111, "0x1f4", 1000033, 100000],
"id": "getblock.io"}
```

Response

```json
{"jsonrpc":"2.0","id":1337,"result":{"transaction":{"visible":false,"txID":"ae02a80abd985a6f05478b9bbf04706f00cdbf71e38c77d21ed77e44c634cef9","raw_data":{"contract":[{"parameter":{"value":{"amount":500,"owner_address":"41c4db2c9dfbcb6aa344793f1dda7bd656598a06d8","to_address":"4195fd23d3d2221cfef64167938de5e62074719e54"},"type_url":"type.googleapis.com/protocol.TransferContract"},"type":"TransferContract"}],"ref_block_bytes":"957e","ref_block_hash":"3922d8c0d28b5283","expiration":1684469286000,"timestamp":1684469226841},"raw_data_hex":"0a02957e22083922d8c0d28b528340f088c69183315a66080112620a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412310a1541c4db2c9dfbcb6aa344793f1dda7bd656598a06d812154195fd23d3d2221cfef64167938de5e62074719e5418f40370d9bac2918331"}}}
```

## Body Parameters

Here is the list of body parameters for the buildTransaction method:

| Field   | Description                                                                 |
| ------- | --------------------------------------------------------------------------- |
| jsonrpc | JSON-RPC protocol version (usually `"2.0"`).                                |
| id      | An identifier for the request, used to match the response with the request. |
| result  | The main object containing the transaction data.                            |

#### result.transaction:

| Field          | Description                                                                           |
| -------------- | ------------------------------------------------------------------------------------- |
| visible        | Boolean indicating if addresses are presented in base58 (true) or hex (false) format. |
| txID           | Transaction ID (hash).                                                                |
| raw\_data      | Raw transaction data (contains all transaction details).                              |
| raw\_data\_hex | Hex-encoded raw data of the transaction.                                              |

## Use Case

Here are some use-cases for the `buildTransaction` method:

1. **Token Transfer Automation**: The `buildTransaction` method can be used to automate the process of transferring tokens between accounts on a blockchain network. By specifying parameters such as the recipient address, token ID, and amount, developers can programmatically initiate token transfers without manual intervention. This is particularly useful for applications that require frequent or scheduled token transfers, such as payroll systems or subscription services.
2. **Smart Contract Interaction**: In Web3 programming, interacting with smart contracts often involves building and sending transactions to execute specific functions. The `buildTransaction` method allows developers to construct transactions that call smart contract methods, such as `TransferTokenTo` or `getResultInCon`, with the required input parameters. This facilitates seamless integration with decentralized applications (dApps) that rely on smart contract logic for operations like voting, staking, or decentralized finance (DeFi) activities.
3. **Custom Transaction Creation**: Developers can use the `buildTransaction` method to create custom transactions tailored to specific use cases. By providing a bytecode or ABI (Application Binary Interface) and other transaction details, developers can deploy new smart contracts or execute complex operations on the blockchain. This flexibility is essential for building innovative blockchain solutions that require bespoke transaction logic, such as multi-signature wallets or cross-chain bridges.

## Code for buildTransaction

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {"jsonrpc": "2.0",
"method": "buildTransaction",
"params": ["0xC4DB2C9DFBCB6AA344793F1DDA7BD656598A06D8", "transferTokenContract", "0x245498", "[{\"constant\":false,\"inputs\":[],\"name\":\"getResultInCon\",\"outputs\":[{\"name\":\"\",\"type\":\"trcToken\"},{\"name\":\"\",\"type\":\"uint256\"},{\"name\":\"\",\"type\":\"uint256\"}],\"payable\":true,\"stateMutability\":\"payable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"name\":\"toAddress\",\"type\":\"address\"},{\"name\":\"id\",\"type\":\"trcToken\"},{\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"TransferTokenTo\",\"outputs\":[],\"payable\":true,\"stateMutability\":\"payable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[],\"name\":\"msgTokenValueAndTokenIdTest\",\"outputs\":[{\"name\":\"\",\"type\":\"trcToken\"},{\"name\":\"\",\"type\":\"uint256\"},{\"name\":\"\",\"type\":\"uint256\"}],\"payable\":true,\"stateMutability\":\"payable\",\"type\":\"function\"},{\"inputs\":[],\"payable\":true,\"stateMutability\":\"payable\",\"type\":\"constructor\"}]\n", "6080604052d3600055d2600155346002556101418061001f6000396000f3006080604052600436106100565763ffffffff7c010000000000000000000000000000000000000000000000000000000060003504166305c24200811461005b5780633be9ece71461008157806371dc08ce146100aa575b600080fd5b6100636100b2565b60408051938452602084019290925282820152519081900360600190f35b6100a873ffffffffffffffffffffffffffffffffffffffff600435166024356044356100c0565b005b61006361010d565b600054600154600254909192565b60405173ffffffffffffffffffffffffffffffffffffffff84169082156108fc029083908590600081818185878a8ad0945050505050158015610107573d6000803e3d6000fd5b50505050565bd3d2349091925600a165627a7a72305820a2fb39541e90eda9a2f5f9e7905ef98e66e60dd4b38e00b05de418da3154e7570029", 100, 11111111111111, "0x1f4", 1000033, 100000],
"id": "getblock.io"}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)
```

## Common Errors

**Common Errors**\
When using the buildTransaction RPC Tron method, the following issues may occur:

* **Invalid Address Format**: The recipient address may not be in the correct format. Ensure that the address is a valid hexadecimal string starting with '0x' and is 42 characters long.
* **Insufficient Balance**: If the sender's account lacks sufficient funds to cover the transaction amount and fees, the transaction will fail. Verify the account balance before initiating the transaction.
* **Incorrect Parameter Types**: Parameters such as token IDs or amounts must match the expected data types. Double-check the types and format of each parameter to ensure compatibility with the method’s requirements.
* **Gas Limit Too Low**: Setting a gas limit that is too low can cause the transaction to fail. Estimate the required gas accurately by analyzing similar transactions or using estimation tools.

Using the buildTransaction method in Web3 applications is beneficial as it allows developers to construct and customize transactions programmatically, enhancing the automation and efficiency of blockchain interactions. This method provides a flexible way to prepare transactions before broadcasting, ensuring that all necessary parameters are correctly set and optimized for successful execution on the Tron network.

### conclusion

The buildTransaction RPC method on the Tron network facilitates the creation of transactions by specifying parameters such as contract addresses, method names, and encoded function calls. This process is crucial for executing smart contract interactions and token transfers, ensuring seamless blockchain operations. By leveraging buildTransaction, developers can efficiently construct and deploy transactions on the Tron blockchain.
