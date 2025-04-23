# deploycontract


## Meta Description
Deploy smart contracts using the 'deploycontract' method via Tron’s HTTP REST API Interface.

## Description
The 'deploycontract' method in the Tron protocol is a key feature of the deploycontract Web3 interface, allowing developers to deploy smart contracts on the Tron blockchain seamlessly. This method is part of the deploycontract HTTP REST API protocol, providing a structured approach to initiate and manage smart contract deployment through the Tron network. By utilizing the HTTP REST API Interface, users can send a contract creation request, specifying the bytecode and constructor parameters. This interaction ensures a streamlined, efficient process for deploying decentralized applications, making it easier for developers to harness the full potential of Tron’s robust blockchain environment. With clear, precise commands, 'deploycontract' empowers users to bring their blockchain innovations to life.

## Supported Networks
The deploycontract HTTP REST API method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters deploycontract method needs to be executed.

- **abi** (required): 
  - **Type**: String
  - **Description**: The Application Binary Interface (ABI) of the smart contract, which defines the methods and structures within the contract.
  - **Default/Supported Values**: JSON string representing ABI.

- **bytecode** (required): 
  - **Type**: String
  - **Description**: The compiled bytecode of the smart contract, which is deployed to the blockchain.
  - **Default/Supported Values**: Hexadecimal string.

- **owner_address** (required): 
  - **Type**: String
  - **Description**: The address of the account that will own the deployed contract.
  - **Default/Supported Values**: Valid blockchain address format.

- **name** (required): 
  - **Type**: String
  - **Description**: The name of the smart contract.
  - **Default/Supported Values**: Any string representing the contract's name.

- **visible** (optional): 
  - **Type**: Boolean
  - **Description**: Indicates whether the contract should be visible on the blockchain explorer.
  - **Default/Supported Values**: `true` or `false`. Default is often `true`.

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using deploycontract

Request
```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/deploycontract \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "abi": "[{\"constant\":false,\"inputs\":[{\"name\":\"key\",\"type\":\"uint256\"},{\"name\":\"value\",\"type\":\"uint256\"}],\"name\":\"set\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"name\":\"key\",\"type\":\"uint256\"}],\"name\":\"get\",\"outputs\":[{\"name\":\"value\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"}]",
  "bytecode": "608060405234801561001057600080fd5b5060de8061001f6000396000f30060806040526004361060485763ffffffff7c01000000000000000000000000000000000000000000000000000000006000350416631ab06ee58114604d5780639507d39a146067575b600080fd5b348015605857600080fd5b506065600435602435608e565b005b348015607257600080fd5b50607c60043560a0565b60408051918252519081900360200190f35b60009182526020829052604090912055565b600090815260208190526040902054905600a165627a7a72305820fdfe832221d60dd582b4526afa20518b98c2e1cb0054653053a844cf265b25040029",
  "owner_address": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh",
  "name": "SomeContract",
  "visible": true
}
'
```

Response
```json
{
  "visible": true,
  "txID": "bb9cddd37313752581ffd0a33a202ce20fc2a092cebf72101cfc54ed76e94ad9",
  "contract_address": "41e082d7113aa0e8ab2c5483cb803616fb3a1d76b6",
  "raw_data": {
    "contract": [
      {
        "parameter": {
          "value": {
            "owner_address": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh",
            "new_contract": {
              "bytecode": "608060405234801561001057600080fd5b5060de8061001f6000396000f30060806040526004361060485763ffffffff7c01000000000000000000000000000000000000000000000000000000006000350416631ab06ee58114604d5780639507d39a146067575b600080fd5b348015605857600080fd5b506065600435602435608e565b005b348015607257600080fd5b50607c60043560a0565b60408051918252519081900360200190f35b60009182526020829052604090912055565b600090815260208190526040902054905600a165627a7a72305820fdfe832221d60dd582b4526afa20518b98c2e1cb0054653053a844cf265b25040029",
              "name": "SomeContract",
              "origin_address": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh",
              "abi": {
                "entrys": [
                  {
                    "inputs": [
                      {
                        "name": "key",
                        "type": "uint256"
                      },
                      {
                        "name": "value",
                        "type": "uint256"
                      }
                    ],
                    "name": "set",
                    "stateMutability": "Nonpayable",
                    "type": "Function"
                  },
                  {
                    "outputs": [
                      {
                        "name": "value",
                        "type": "uint256"
                      }
                    ],
                    "constant": true,
                    "inputs": [
                      {
                        "name": "key",
                        "type": "uint256"
                      }
                    ],
                    "name": "get",
                    "stateMutability": "View",
                    "type": "Function"
                  }
                ]
              }
            }
          },
          "type_url": "type.googleapis.com/protocol.CreateSmartContract"
        },
        "type": "CreateSmartContract"
      }
    ],
    "ref_block_bytes": "8a86",
    "ref_block_hash": "4ac52ad154123793",
    "expiration": 1745339673000,
    "timestamp": 1745339614395
  },
  "raw_data_hex": "0a028a8622084ac52ad15412379340a8d3e7f2e5325ad703081e12d2030a30747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e437265617465536d617274436f6e7472616374129d030a1541608f8da72479edc7dd921e4c30bb7e7cddbe722e1283030a1541608f8da72479edc7dd921e4c30bb7e7cddbe722e1a5c0a2b1a03736574220e12036b65791a0775696e743235362210120576616c75651a0775696e74323536300240030a2d10011a03676574220e12036b65791a0775696e743235362a10120576616c75651a0775696e743235363002400222fd01608060405234801561001057600080fd5b5060de8061001f6000396000f30060806040526004361060485763ffffffff7c01000000000000000000000000000000000000000000000000000000006000350416631ab06ee58114604d5780639507d39a146067575b600080fd5b348015605857600080fd5b506065600435602435608e565b005b348015607257600080fd5b50607c60043560a0565b60408051918252519081900360200190f35b60009182526020829052604090912055565b600090815260208190526040902054905600a165627a7a72305820fdfe832221d60dd582b4526afa20518b98c2e1cb0054653053a844cf265b250400293a0c536f6d65436f6e747261637470bb89e4f2e532"
}
```
## Body Parameters

Here is the list of body parameters for the deploycontract method:

### Smart Contract Deployment Parameters

1. **abi** (`json`)  
   - **Description**: Smart Contract's Application Binary Interface (ABI).

2. **bytecode** (`string`)  
   - **Description**: The compiled contract's identifier, used to interact with the Virtual Machine.

3. **fee_limit** (`int32`)  
   - **Description**: Maximum TRX consumption, measured in **SUN** (1 TRX = 1,000,000 SUN).  

4. **parameter** (`string`)  
   - **Description**: Parameter passed to the constructor of the contract. Must be converted to the virtual machine format (e.g., use Remix.js tools to transform arrays like `[1, 2]`).  

5. **origin_energy_limit** (`int32`)  
   - **Description**: The max energy consumed by the owner during contract execution/creation. Must be > 0.  

6. **owner_address** (`string`)  
   - **Description**: Contract owner address (hex string).  
   - **Default**: `TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh`.  

7. **name** (`string`)  
   - **Description**: Contract name.  
   - **Default**: `SomeContract`.  

8. **call_value** (`int32`)  
   - **Description**: Amount of TRX transferred with this transaction (in SUN).  

9. **consume_user_resource_percent** (`int32`)  
   - **Description**: User Pay Ratio (0–100). Percentage of resources charged to users.  
     - **Recommended**: `1`–`99` (to prevent infinite loop attacks).  

10. **permission_id** (`int32`) *(Optional)*  
    - **Description**: For multi-signature permissions.  

11. **visible** (`boolean`)  
    - **Description**: Whether addresses are in base58 format.  
    - **Default**: `true`.  


## Use Case

Here are some use-cases for the deploycontract method:

1. **Decentralized Applications (DApps)**: In Web3 programming, deploying a smart contract is essential for creating decentralized applications. By using the deploycontract method, developers can launch their smart contracts on the blockchain, allowing the DApp to operate in a decentralized manner. This is particularly useful for applications that require trustless interactions, such as decentralized finance (DeFi) platforms, where users can lend, borrow, or trade assets without relying on a central authority.

2. **Token Creation**: Another common use case for the deploycontract method is the creation and deployment of tokens on the blockchain. Developers can deploy smart contracts that define the rules and functionalities of tokens, such as ERC-20 or ERC-721 tokens on Ethereum. This is crucial for projects looking to create their own cryptocurrencies or non-fungible tokens (NFTs), enabling them to manage token distribution, transfers, and interactions within their ecosystem.

3. **Automated Agreements**: Smart contracts can automate agreements between parties without the need for intermediaries. By deploying contracts using the deploycontract method, businesses can create automated systems for various use cases, such as supply chain management, where contracts can automatically execute payments or shipments based on predefined conditions. This reduces the need for manual intervention and increases efficiency and trust between parties involved.

## Code for deploycontract


```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/deploycontract"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

data = {
    "abi": "[{\"constant\":false,\"inputs\":[{\"name\":\"key\",\"type\":\"uint256\"},{\"name\":\"value\",\"type\":\"uint256\"}],\"name\":\"set\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"name\":\"key\",\"type\":\"uint256\"}],\"name\":\"get\",\"outputs\":[{\"name\":\"value\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"}]",
    "bytecode": "608060405234801561001057600080fd5b5060de8061001f6000396000f30060806040526004361060485763ffffffff7c01000000000000000000000000000000000000000000000000000000006000350416631ab06ee58114604d5780639507d39a146067575b600080fd5b348015605857600080fd5b506065600435602435608e565b005b348015607257600080fd5b50607c60043560a0565b60408051918252519081900360200190f35b60009182526020829052604090912055565b600090815260208190526040902054905600a165627a7a72305820fdfe832221d60dd582b4526afa20518b98c2e1cb0054653053a844cf265b25040029",
    "owner_address": "TJmmqjb1DK9TTZbQXzRQ2AuA94z4gKAPFh",
    "name": "SomeContract",
    "visible": True
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())

```
## Common Errors

Common Errors  
When using the deploycontract HTTP REST API Tron method, the following issues may occur:  
- **Insufficient Energy or TRX**: Deploying a contract requires a certain amount of energy and TRX. Ensure your account has enough resources by checking your balance and freezing TRX for energy if necessary.  
- **Invalid Bytecode**: If the provided bytecode is not valid or is incorrectly formatted, the deployment will fail. Double-check the bytecode for errors and ensure it matches the compiled output from a trusted compiler.  
- **Incorrect ABI Format**: The ABI must be a valid JSON string. Ensure that the ABI is correctly formatted and does not contain any syntax errors, such as missing brackets or commas.  
- **Unauthorized Address**: The owner address may not have the necessary permissions to deploy contracts. Verify that the address is authorized and has the necessary rights to perform the deployment.

Using the deploycontract method in Web3 applications allows developers to programmatically deploy smart contracts on the Tron network, enabling decentralized applications to dynamically expand their functionality. This method provides a streamlined and efficient process for managing contract deployments, enhancing the scalability and adaptability of blockchain solutions.

### conclusion

Deploying a smart contract like "SomeContract" on the Tron network involves utilizing the Tron HTTP REST API to interact with the blockchain. This process requires the contract's ABI and bytecode, as well as the owner's address, to ensure a successful deployment. By leveraging the deploycontract functionality, developers can efficiently launch and manage decentralized applications on the Tron blockchain.
