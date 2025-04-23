# broadcasttransaction


## Meta Description
Use the 'broadcasttransaction' method in the HTTP REST API Interface to submit transactions on the Tron network efficiently.

## Description
The 'broadcasttransaction' Web3 method in the Tron protocol allows developers to submit transactions to the Tron blockchain via the HTTP REST API Interface. This method is integral for ensuring that transactions are propagated across the network efficiently and securely. By leveraging the 'broadcasttransaction' RPC protocol, developers can interact programmatically with the Tron network, enabling seamless integration into decentralized applications. The method requires a serialized transaction as input and returns the transaction's broadcast status. Designed for reliability and speed, 'broadcasttransaction' is essential for developers aiming to maintain high-performance applications on the Tron network. Its user-friendly interface and robust protocol support make it a preferred choice for blockchain developers.

## Supported Networks
The broadcasttransaction HTTP REST API method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters the broadcasttransaction method needs to be executed:

- **contract** (required, object): Contains the details of the transaction.
  - **parameter** (required, object): Contains the specific details of the transaction parameters.
    - **value** (required, object): Contains the values for the transaction.
      - **amount** (required, integer): The amount to be transferred. No default value.
      - **owner_address** (required, string): The address of the owner initiating the transfer. No default value.
      - **to_address** (required, string): The address of the recipient. No default value.
    - **type_url** (required, string): Specifies the type of contract. Supported value: `"type.googleapis.com/protocol.TransferContract"`.
  - **type** (required, string): The type of contract. Supported value: `"TransferContract"`.

- **ref_block_bytes** (required, string): Reference to the block bytes. No default value.
- **ref_block_hash** (required, string): Reference to the block hash. No default value.
- **expiration** (required, integer): The expiration timestamp of the transaction in milliseconds. No default value.
- **timestamp** (required, integer): The creation timestamp of the transaction in milliseconds. No default value.

This breakdown provides a detailed understanding of each parameter required for the broadcasttransaction method to be executed.

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using broadcasttransaction

Request
```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/broadcasttransaction \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "raw_data": {
    "contract": [
      {
        "parameter": {
          "value": {
            "amount": 1000,
            "owner_address": "41608f8da72479edc7dd921e4c30bb7e7cddbe722e",
            "to_address": "41e9d79cc47518930bc322d9bf7cddd260a0260a8d"
          },
          "type_url": "type.googleapis.com/protocol.TransferContract"
        },
        "type": "TransferContract"
      }
    ],
    "ref_block_bytes": "5e4b",
    "ref_block_hash": "47c9dc89341b300d",
    "expiration": 1591089627000,
    "timestamp": 1591089567635
  },
  "raw_data_hex": "0a025e4b220847c9dc89341b300d40f8fed3a2a72e5a66080112620a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412310a1541608f8da72479edc7dd921e4c30bb7e7cddbe722e121541e9d79cc47518930bc322d9bf7cddd260a0260a8d18e8077093afd0a2a72e"
}
'
```

Response
```json
{
  "code": "TRANSACTION_EXPIRATION_ERROR",
  "txid": "77ddfa7093cc5f745c0d3a54abb89ef070f983343c05e0f89e5a52f3e5401299",
  "message": "5472616e73616374696f6e2065787069726564"
}
```
## Body Parameters

Here is the list of body parameters for the broadcasttransaction method:

1. **code**: This parameter indicates the error code related to the transaction. In this case, it is "TRANSACTION_EXPIRATION_ERROR," which suggests that the transaction has expired.

2. **txid**: This parameter provides the transaction ID, which is a unique identifier for the transaction. In this example, the transaction ID is "77ddfa7093cc5f745c0d3a54abb89ef070f983343c05e0f89e5a52f3e5401299."

3. **message**: This parameter contains a message related to the transaction error. The message is often encoded and needs to be decoded to read the human-readable text. In this example, the message "5472616e73616374696f6e2065787069726564" translates to "Transaction expired" in ASCII.

## Use Case

Here are some use-cases for the `broadcastTransaction` method in Web3 programming:

1. **Token Transfers**: The `broadcastTransaction` method is commonly used for token transfers on blockchain networks. In the provided JSON data, a `TransferContract` is used to transfer a specific amount of tokens from one address to another. Developers can utilize this method to programmatically send tokens between accounts, enabling functionalities such as payments, rewards, or automated disbursements in decentralized applications (dApps).

2. **Smart Contract Interactions**: Beyond simple token transfers, the `broadcastTransaction` method can be used to interact with smart contracts. This includes executing functions within a smart contract, updating contract state, or triggering specific contract events. Developers can create complex decentralized applications that require interaction with smart contracts, utilizing this method to ensure transactions are properly propagated across the network.

3. **Decentralized Finance (DeFi) Operations**: In the DeFi space, users often need to perform operations such as swapping tokens, staking, or providing liquidity. The `broadcastTransaction` method facilitates these operations by broadcasting the necessary transactions to the blockchain network. By leveraging this method, developers can build financial applications that allow users to participate in DeFi protocols, manage their assets, and execute transactions in a trustless and decentralized manner.

## Code for broadcasttransaction


```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/broadcasttransaction"

payload = {
    "raw_data": {
        "contract": [
            {
                "parameter": {
                    "value": {
                        "amount": 1000,
                        "owner_address": "41608f8da72479edc7dd921e4c30bb7e7cddbe722e",
                        "to_address": "41e9d79cc47518930bc322d9bf7cddd260a0260a8d"
                    },
                    "type_url": "type.googleapis.com/protocol.TransferContract"
                },
                "type": "TransferContract"
            }
        ],
        "ref_block_bytes": "5e4b",
        "ref_block_hash": "47c9dc89341b300d",
        "expiration": 1591089627000,
        "timestamp": 1591089567635
    },
    "raw_data_hex": "0a025e4b220847c9dc89341b300d40f8fed3a2a72e5a66080112620a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412310a1541608f8da72479edc7dd921e4c30bb7e7cddbe722e121541e9d79cc47518930bc322d9bf7cddd260a0260a8d18e8077093afd0a2a72e"
}

headers = {
    "accept": "application/json",
    "content-type": "application/json"
}

response = requests.post(url, json=payload, headers=headers)

print(response.status_code)
print(response.text)

```
## Common Errors

Common Errors  
When using the broadcasttransaction HTTP REST API Tron method, the following issues may occur:  
- **Invalid Signature**: If the transaction signature is incorrect or missing, the network will reject it. Ensure that the private key used to sign the transaction corresponds to the owner address and that the signature is correctly applied.  
- **Insufficient Balance**: Transactions may fail if the owner's account does not have enough TRX to cover the transaction amount and associated fees. Verify the account balance before initiating the transaction to prevent this error.  
- **Expired Transaction**: Transactions that exceed their expiration time will not be processed. Check the transaction's expiration timestamp and ensure it is set appropriately to allow sufficient time for network propagation.  
- **Network Congestion**: High network traffic can delay transaction processing or cause it to fail. Consider increasing the transaction fee to prioritize processing or retry broadcasting during off-peak times.  

The broadcasttransaction method is crucial for Web3 applications as it facilitates the secure and efficient transfer of assets on the Tron network. By enabling decentralized transactions, it supports the creation of trustless systems and empowers developers to build innovative blockchain-based solutions.

### conclusion

The provided data represents a transaction on the Tron blockchain, specifically a TransferContract, where 1000 units are being transferred from one address to another. The transaction details, including block references and timestamps, are crucial for ensuring its validity and proper inclusion in the blockchain. To finalize and propagate this transaction across the network, one would typically use the broadcasttransaction HTTP REST API Tron call, which ensures that the transaction is broadcasted to all nodes for validation and inclusion in the next block.
