# createtransaction


## Meta Description
The 'createtransaction' method in Tron's HTTP REST API Interface enables seamless transaction creation with precision and efficiency.

## Description
The 'createtransaction' Web3 method in the Tron protocol is a pivotal component of the createtransaction HTTP REST API protocol, designed for developers to initiate transactions seamlessly within the Tron blockchain environment. This method facilitates the construction of a transaction object, specifying crucial details such as the sender and receiver addresses, the amount to be transferred, and any additional parameters required for customization. By leveraging the HTTP REST API Interface, developers can ensure precision and reliability in transaction creation, enabling efficient interaction with the Tron network. The 'createtransaction' method is an essential tool for developers aiming to build robust applications on Tron, offering a streamlined approach to managing blockchain transactions.

## Supported Networks
The createtransaction HTTP REST API method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters the createtransaction method needs to be executed.

- **owner_address** (required)
  - **Type**: String
  - **Description**: The address of the owner initiating the transaction.
  - **Default/Supported Values**: A valid blockchain address format.

- **to_address** (required)
  - **Type**: String
  - **Description**: The recipient's address to which the transaction amount will be sent.
  - **Default/Supported Values**: A valid blockchain address format.

- **amount** (required)
  - **Type**: Integer
  - **Description**: The amount of currency to be transferred in the transaction.
  - **Default/Supported Values**: A positive integer representing the transaction amount.

- **visible** (optional)
  - **Type**: Boolean
  - **Description**: A flag indicating whether the transaction should be visible.
  - **Default/Supported Values**: `true` or `false`. Default is typically `false` if not specified.

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using createtransaction

Request
```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/createtransaction \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "to_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
  "amount": 1000,
  "visible": true
}
'
```

Response
```json
{
  "visible": true,
  "txID": "9b030b1d4eea348d06ee78020ff3662762061fb2ed2c2197a9ba1f6f0b38087e",
  "raw_data": {
    "contract": [
      {
        "parameter": {
          "value": {
            "amount": 1000,
            "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
            "to_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1"
          },
          "type_url": "type.googleapis.com/protocol.TransferContract"
        },
        "type": "TransferContract"
      }
    ],
    "ref_block_bytes": "8907",
    "ref_block_hash": "2a3218d89b765661",
    "expiration": 1745338512000,
    "timestamp": 1745338454052
  },
  "raw_data_hex": "0a02890722082a3218d89b7656614080e5a0f2e5325a66080112620a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412310a1541fd49eda0f23ff7ec1d03b52c3a45991c24cd440e12154198927ffb9f554dc4a453c64b2e553a02d6df514b18e80770a4a09df2e532"
}
```
## Body Parameters

Here is the list of body parameters for the `createtransaction` method:

1. **owner_address**  
   - Type: String  
   - Description: Defaults to `TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g`. Owner_address is the transfer address, converted to a hex string.  
   - Example Value: `TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g`

2. **to_address**  
   - Type: String  
   - Description: Defaults to `TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1`. To_address is the transfer address, converted to a hex string.  
   - Example Value: `TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1`

3. **amount**  
   - Type: Int64  
   - Description: Defaults to `1000`. Amount is the transfer amount, and the unit is `sun`.  
   - Example Value: `1000`

4. **permission_id**  
   - Type: Int32  
   - Description: Optional, for multi-signature use.  
   - Example Value: `1234`

5. **visible**  
   - Type: Boolean  
   - Description: Defaults to `true`. Whether the address is in base58 format.  
   - Example Value: `true`

6. **extra_data**  
   - Type: String  
   - Description: Optional, notes on the transaction, HEX format.  
   - Example Value: `0x4e6f746573`


## Use Case

Here are some use-cases for the `createtransaction` method in Web3 programming:

1. **Peer-to-Peer Transfers**: The `createtransaction` method is essential for facilitating direct peer-to-peer transfers of cryptocurrency. For example, if a user wants to send 1000 tokens from their wallet to another user's wallet, this method can be used to create a transaction that specifies the sender's address, the recipient's address, and the amount to be transferred. This is a fundamental operation in decentralized finance (DeFi) applications where users need to exchange assets without intermediaries.

2. **Smart Contract Interactions**: Another use case for the `createtransaction` method is interacting with smart contracts. When a user wants to execute a function on a smart contract, such as participating in a decentralized exchange or staking tokens in a yield farming protocol, a transaction needs to be created and sent to the blockchain. This transaction would include the contract address, the function to be called, and any necessary parameters, including the amount of cryptocurrency involved in the operation.

3. **Token Transfers and Management**: Web3 applications often involve managing and transferring various types of tokens. The `createtransaction` method can be used to handle these operations, such as transferring ERC-20 tokens on the Ethereum blockchain. This is particularly useful for applications like wallets or portfolio trackers, where users need to manage their token holdings efficiently, ensuring that transactions are properly created and broadcasted to the network.

## Code for createtransaction


```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/createtransaction"

headers = {
    "accept": "application/json",
    "content-type": "application/json"
}

payload = {
    "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
    "to_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
    "amount": 1000,
    "visible": True
}

response = requests.post(url, json=payload, headers=headers)

print(response.status_code)
print(response.json())
```
## Common Errors

Common Errors  
When using the createtransaction HTTP REST API Tron method, the following issues may occur:  
- Invalid Address Format: Ensure that both the owner and recipient addresses are valid Tron addresses, typically starting with a 'T', to avoid transaction failures.  
- Insufficient Balance: Verify that the owner's account has enough TRX to cover the transaction amount and any associated fees to prevent rejection.  
- Incorrect Amount Value: Double-check that the amount is a positive integer and within the permissible limits to ensure successful transaction processing.  
- Visibility Parameter Misuse: The 'visible' parameter should be used correctly according to the API specifications, as it may affect how the transaction is broadcasted or displayed.  

Using the createtransaction method in Web3 applications facilitates seamless and programmatic fund transfers on the Tron network. It empowers developers to integrate decentralized financial transactions directly into their applications, enhancing user experience and enabling more complex blockchain-based functionalities.

### conclusion

The provided HTTP REST API object outlines the parameters for initiating a transaction on the Tron network using the createtransaction HTTP REST API method. This method facilitates the transfer of 1000 TRX from the owner address "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g" to the recipient address "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1". By utilizing the createtransaction HTTP REST API Tron command, users can seamlessly execute and manage transactions on the blockchain.
