---
description: >-
  scanshieldedtrc20notesbyivk RESTful API Interface for Tron protocol's shielded
  TRC20 transactions.
---

# scanshieldedtrc20notesbyivk - TRON

## Description

The scanshieldedtrc20notesbyivk Web3 method within the Tron protocol provides a robust mechanism for scanning shielded TRC20 notes using an incoming viewing key (IVK). This RESTful API Interface allows developers to efficiently interact with Tron’s privacy-focused features, enabling the retrieval of shielded transaction details securely and privately. Utilizing the scanshieldedtrc20notesbyivk RPC protocol, users can seamlessly integrate shielded TRC20 transaction scanning capabilities into their applications, ensuring that sensitive transaction data remains confidential. This method is ideal for developers seeking to enhance privacy and security in their blockchain applications while maintaining ease of use and integration.

## Supported Networks

The scanshieldedtrc20notesbyivk REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters scanshieldedtrc20notesbyivk method needs to be executed.

* **owner\_address** (Required)
  * **Type**: String
  * **Description**: The address of the owner associated with the shielded TRC-20 notes.
  * **Supported Values**: A valid address string, e.g., "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g".
* **exchange\_id** (Required)
  * **Type**: Integer
  * **Description**: The identifier for the exchange being referenced.
  * **Supported Values**: Any valid integer, e.g., 12.
* **token\_id** (Required)
  * **Type**: String
  * **Description**: The unique identifier for the token.
  * **Supported Values**: A valid token ID string, e.g., "31303030343837".
* **quant** (Required)
  * **Type**: Integer
  * **Description**: The quantity of the token being referenced.
  * **Supported Values**: Any valid integer, e.g., 100.
* **expected** (Required)
  * **Type**: Integer
  * **Description**: The expected value or outcome for the operation.
  * **Supported Values**: Any valid integer, e.g., 10.
* **visible** (Optional)
  * **Type**: Boolean
  * **Description**: A flag indicating whether the transaction should be visible.
  * **Default Value**: true
  * **Supported Values**: true or false.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using scanshieldedtrc20notesbyivk

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/ \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "exchange_id": 12,
  "token_id": "31303030343837",
  "quant": 100,
  "expected": 10,
  "visible": true
}
```

Response

```json
{
  "Error": "class org.tron.core.exception.BadItemException : request requires start_block_index >= 0 && end_block_index > start_block_index && end_block_index - start_block_index <= 1000"
}
```

## Body Parameters

Here is the list of body parameters for the `scanshieldedtrc20notesbyivk` method:

1. **start\_block\_index**: The starting block index for the scan. This parameter must be greater than or equal to 0.
2. **end\_block\_index**: The ending block index for the scan. This parameter must be greater than the `start_block_index`.
3. **ivk**: The incoming viewing key that is used to scan for shielded TRC20 notes.
4. **limit**: The maximum number of notes to return. This parameter helps in controlling the size of the response.
5. **offset**: The number of notes to skip before starting to collect the response set. This is useful for pagination.
6. **include\_spent**: A boolean parameter indicating whether to include spent notes in the response. Default is typically `false`.
7. **include\_memo**: A boolean parameter indicating whether to include memo data in the response. Default is typically `false`.

These parameters are designed to help you effectively query and manage shielded TRC20 notes within a specified block range.

## Use Case

Here are some use-cases for the `scanshieldedtrc20notesbyivk` method in Web3 programming:

1. **Privacy-Preserving Transactions**: In Web3 applications, privacy is a critical concern for users who wish to keep their transaction details confidential. The `scanshieldedtrc20notesbyivk` method can be utilized to scan and identify shielded TRC20 notes associated with a particular viewing key (IVK). This allows developers to build applications that enable users to verify their transactions without revealing sensitive information to the public blockchain. Such functionality is particularly beneficial for privacy-focused decentralized finance (DeFi) platforms where users demand confidentiality in their financial activities.
2. **Enhanced Security for Token Transfers**: Another use case for the `scanshieldedtrc20notesbyivk` method is enhancing the security of token transfers within a Web3 ecosystem. By using this method, developers can create secure wallets that can detect and manage shielded token balances. This ensures that users can safely store and transfer tokens without exposing their transaction history or balance to potential attackers. This feature is crucial for users who prioritize security and wish to protect their assets from malicious actors.
3. **Compliance and Auditing**: While privacy is essential, there are scenarios where compliance and auditing are necessary, such as in regulated environments. The `scanshieldedtrc20notesbyivk` method can be integrated into compliance tools to allow authorized parties to verify transactions and balances without compromising user privacy. This enables organizations to meet regulatory requirements while maintaining the confidentiality of their users' data, striking a balance between transparency and privacy.

## Code for scanshieldedtrc20notesbyivk

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "exchange_id": 12,
  "token_id": "31303030343837",
  "quant": 100,
  "expected": 10,
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

**Common Errors**\
When using the scanshieldedtrc20notesbyivk HTTP REST API Tron method, the following issues may occur:

* **Invalid Owner Address**: If the owner\_address is not a valid Tron address, the request will fail. Ensure the address is correctly formatted and belongs to the Tron network.
* **Incorrect Token ID Format**: The token\_id must be in the correct hexadecimal format. Double-check the token ID for any typographical errors or incorrect encoding.
* **Exchange ID Mismatch**: An incorrect exchange\_id can lead to data retrieval issues. Verify that the exchange\_id corresponds to the correct exchange or service being queried.
* **Visibility Parameter Misconfiguration**: If the visible parameter is set incorrectly, it may lead to unexpected results. Confirm that this boolean value aligns with your intended visibility settings for the operation.

Using the scanshieldedtrc20notesbyivk method in Web3 applications provides enhanced privacy and security by allowing developers to interact with shielded TRC-20 tokens. This functionality supports the creation of decentralized applications that require confidential transactions, fostering a more secure and private blockchain ecosystem.

### conclusion

The provided JSON data appears to be a transaction or asset-related entry, possibly involving a TRC20 token on the Tron blockchain. The "scanshieldedtrc20notesbyivk" HTTP API on Tron might be used to verify or track such transactions securely, ensuring privacy and transparency. Utilizing this API can help users manage their digital assets effectively while maintaining the confidentiality of their transaction details.
