# broadcasthex


## Meta Description
Explore the 'broadcasthex' method in Tron's HTTP REST API Interface for broadcasting transactions efficiently.

## Description
The 'broadcasthex' method in the Tron protocol's Web3 interface allows users to broadcast signed transactions in hexadecimal format across the network. As part of the broadcasthex HTTP REST API protocol, this method facilitates the propagation of transaction data, ensuring it reaches all nodes for validation and inclusion in the blockchain. By leveraging the HTTP REST API Interface, developers can integrate this functionality into their applications to seamlessly manage transaction submissions. The 'broadcasthex' Web3 method is essential for developers looking to interact programmatically with the Tron network, offering a reliable way to ensure transactions are promptly and securely broadcasted. Whether for deploying smart contracts or transferring TRX, 'broadcasthex' provides a critical link in the transaction lifecycle.

## Supported Networks
The broadcasthex HTTP REST API method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

None: This method does not require any parameters.

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using broadcasthex

Request
```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/broadcasthex \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "transaction": "0A8A010A0202DB2208C89D4811359A28004098A4E0A6B52D5A730802126F0A32747970652E676F6F676C65617069732E636F6D2F70726F746F636F6C2E5472616E736665724173736574436F6E747261637412390A07313030303030311215415A523B449890854C8FC460AB602DF9F31FE4293F1A15416B0580DA195542DDABE288FEC436C7D5AF769D24206412418BF3F2E492ED443607910EA9EF0A7EF79728DAAAAC0EE2BA6CB87DA38366DF9AC4ADE54B2912C1DEB0EE6666B86A07A6C7DF68F1F9DA171EEE6A370B3CA9CBBB00"
}
'
```

Response
```json
{
  "result": false,
  "code": "TRANSACTION_EXPIRATION_ERROR",
  "txid": "38a0482d6d5a7d1439a50b848d68cafa7d904db48b82344f28765067a5773e1d",
  "message": "Transaction expired",
  "transaction": "{\"raw_data\": {\"ref_block_bytes\": \"02db\",\"ref_block_hash\": \"c89d4811359a2800\",\"expiration\": 1560496575000,\"contract\": [{\"type\": \"TransferAssetContract\",\"parameter\": {\"type_url\": \"type.googleapis.com/protocol.TransferAssetContract\",\"value\": \"0a07313030303030311215415a523b449890854c8fc460ab602df9f31fe4293f1a15416b0580da195542ddabe288fec436c7d5af769d242064\"}}]},\"signature\": [\"8bf3f2e492ed443607910ea9ef0a7ef79728daaaac0ee2ba6cb87da38366df9ac4ade54b2912c1deb0ee6666b86a07a6c7df68f1f9da171eee6a370b3ca9cbbb00\"]}"
}
```
## Body Parameters

Here is the list of body parameters for the broadcasthex method:

1. **result**: A boolean value indicating the success or failure of the transaction. In this case, it is `false`, indicating that the transaction was not successful.

2. **code**: A string that provides an error code associated with the transaction failure. Here, it is `"TRANSACTION_EXPIRATION_ERROR"`, which suggests that the transaction could not be processed because it expired.

3. **txid**: A string representing the transaction ID. This is a unique identifier for the transaction, which in this example is `"38a0482d6d5a7d1439a50b848d68cafa7d904db48b82344f28765067a5773e1d"`.

4. **message**: A string that provides a human-readable explanation of the error. In this case, the message is `"Transaction expired"`, indicating the reason for the transaction's failure.

5. **transaction**: A string containing the raw transaction data in JSON format. This includes details such as the reference block bytes, reference block hash, expiration time, and contract type. The provided example includes a `TransferAssetContract` with specific parameters.

## Use Case

Here are some use-cases for the `broadcasthex` method in Web3 programming:

1. **Transaction Broadcasting:**
   The `broadcasthex` method can be used to broadcast a raw transaction to the blockchain network. This is particularly useful in scenarios where a developer has constructed a transaction offline and needs to send it to the network for validation and inclusion in a block. By using the `broadcasthex` method, developers can ensure that their transaction is propagated across the network and eventually mined.

2. **Cross-Chain Interactions:**
   In a multi-chain environment, the `broadcasthex` method can facilitate cross-chain interactions by allowing transactions to be sent from one blockchain to another. For example, a user might need to move assets from an Ethereum-based blockchain to a Binance Smart Chain. By encoding the transaction data in hexadecimal format and using the `broadcasthex` method, the transaction can be seamlessly transmitted between different blockchain networks.

3. **Automated Transaction Processing:**
   The `broadcasthex` method is also beneficial for automated systems that need to process transactions without manual intervention. For instance, decentralized applications (dApps) or smart contracts might need to initiate transactions based on certain conditions or triggers. By utilizing the `broadcasthex` method, these automated systems can efficiently send transactions to the blockchain, ensuring timely execution of operations such as token transfers, contract interactions, or other blockchain-based activities.

## Code for broadcasthex


```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/broadcasthex"

payload = {
    "transaction": "0A8A010A0202DB2208C89D4811359A28004098A4E0A6B52D5A730802126F0A32747970652E676F6F676C65617069732E636F6D2F70726F746F636F6C2E5472616E736665724173736574436F6E747261637412390A07313030303030311215415A523B449890854C8FC460AB602DF9F31FE4293F1A15416B0580DA195542DDABE288FEC436C7D5AF769D24206412418BF3F2E492ED443607910EA9EF0A7EF79728DAAAAC0EE2BA6CB87DA38366DF9AC4ADE54B2912C1DEB0EE6666B86A07A6C7DF68F1F9DA171EEE6A370B3CA9CBBB00"
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
When using the broadcasthex HTTP REST API Tron method, the following issues may occur:  
- Invalid Transaction Data: If the hexadecimal string is malformed or contains invalid characters, the transaction will be rejected. Ensure the hex string is correctly formatted and adheres to Tron protocol specifications.  
- Network Congestion: During periods of high network activity, transactions may experience delays or fail to broadcast. Consider increasing the transaction fee to prioritize processing.  
- Insufficient Balance: Transactions will fail if the originating account lacks sufficient TRX to cover the transaction and associated fees. Verify account balances before broadcasting transactions.  
- Node Synchronization Issues: If the node you're connected to is not fully synchronized with the Tron network, it may not process transactions correctly. Connect to a reliable and fully synchronized node to ensure successful broadcasting.

Using the broadcasthex method in Web3 applications offers the advantage of directly interacting with the Tron blockchain through raw transaction data, allowing for greater control and flexibility. This method is particularly beneficial for developers looking to implement custom transaction logic or optimize transaction processing in decentralized applications.

### conclusion

The `broadcasthex` HTTP REST API method in Tron is a crucial tool for broadcasting signed transactions to the network. By utilizing `broadcasthex`, developers can ensure that their transactions are efficiently propagated across the Tron blockchain, enhancing the speed and reliability of their decentralized applications. This method is integral for maintaining seamless and secure transaction flows within the Tron ecosystem.
