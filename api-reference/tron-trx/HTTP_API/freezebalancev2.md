---
description: >-
  Use freezebalancev2 via Tron’s RESTful API Interface for efficient balance
  freezing.
---

# freezebalancev2 - TRON

## Description

The 'freezebalancev2' Web3 method in the Tron protocol allows developers to freeze a specified amount of TRX tokens to gain bandwidth or energy, which are essential for executing smart contracts and transactions. This method is accessible through the 'freezebalancev2' RPC protocol, offering a seamless integration for developers looking to optimize resource allocation on the Tron network. By engaging with this RESTful API Interface, users can efficiently manage their token resources, enhance transaction speeds, and reduce costs. The 'freezebalancev2' method is designed to be user-friendly while maintaining a high degree of technical precision, ensuring a smooth experience for developers and users alike.

## Supported Networks

The freezebalancev2 REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters freezebalancev2 method needs to be executed.

* **owner\_address** (required)
  * **Type**: String
  * **Description**: The address of the account that owns the balance to be frozen.
  * **Supported Values**: A valid blockchain address, e.g., "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g".
* **frozen\_balance** (required)
  * **Type**: Integer
  * **Description**: The amount of balance to be frozen.
  * **Supported Values**: A positive integer, e.g., 10000000.
* **resource** (required)
  * **Type**: String
  * **Description**: The type of resource for which the balance is being frozen.
  * **Supported Values**: "ENERGY".
* **visible** (optional)
  * **Type**: Boolean
  * **Description**: Indicates whether the transaction should be visible in the blockchain explorer.
  * **Default Value**: true
  * **Supported Values**: true, false.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using freezebalancev2

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/freezebalancev2 \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "frozen_balance": 10000000,
  "resource": "ENERGY",
  "visible": true
}
```

Response

```json

{
  "visible": true,
  "txID": "0483fe26daa5058fc19f18d5caddf800cabff85ca8bf51353728c3dba629172f",
  "raw_data": {
    "contract": [
      {
        "parameter": {
          "value": {
            "resource": "ENERGY",
            "frozen_balance": 10000000,
            "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g"
          },
          "type_url": "type.googleapis.com/protocol.FreezeBalanceV2Contract"
        },
        "type": "FreezeBalanceV2Contract"
      }
    ],
    "ref_block_bytes": "f3ba",
    "ref_block_hash": "0f1ae3861084f9f9",
    "expiration": 1745341548000,
    "timestamp": 1745341488609
  },
  "raw_data_hex": "0a02f3ba22080f1ae3861084f9f940e08bdaf3e5325a5a083612560a34747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e467265657a6542616c616e63655632436f6e7472616374121e0a1541fd49eda0f23ff7ec1d03b52c3a45991c24cd440e1080ade204180170e1bbd6f3e532"
}
```

## Body Parameters

Here is the list of body parameters for the FreezeBalanceV2 method:

1. **visible**: A boolean indicating whether the transaction is visible or not. In this case, it is set to `true`.
2. **txID**: The transaction ID, which is a unique identifier for the transaction. For this transaction, it is `"0483fe26daa5058fc19f18d5caddf800cabff85ca8bf51353728c3dba629172f"`.
3. **raw\_data**: An object containing the raw data of the transaction:
   * **contract**: A list of contracts involved in the transaction. In this case, it includes:
     * **parameter**: An object containing the details of the contract:
       * **value**: An object with the following properties:
         * **resource**: The type of resource being frozen. Here, it is `"ENERGY"`.
         * **frozen\_balance**: The amount of balance being frozen, expressed in the smallest unit of the currency. Here, it is `10000000`.
         * **owner\_address**: The address of the owner who is freezing the balance. Here, it is `"TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g"`.
       * **type\_url**: The URL indicating the type of contract. Here, it is `"type.googleapis.com/protocol.FreezeBalanceV2Contract"`.
     * **type**: The type of contract, which is `"FreezeBalanceV2Contract"` in this case.
   * **ref\_block\_bytes**: A reference to the block bytes, `"f3ba"` in this case.
   * **ref\_block\_hash**: A reference to the block hash, `"0f1ae3861084f9f9"` in this case.
   * **expiration**: The expiration time of the transaction in milliseconds since the Unix epoch, `1745341548000` in this case.
   * **timestamp**: The timestamp of when the transaction was created, in milliseconds since the Unix epoch, `1745341488609` in this case.
4. **raw\_data\_hex**: A hexadecimal string representation of the raw data, `"0a02f3ba22080f1ae3861084f9f940e08bdaf3e5325a5a083612560a34747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e467265657a6542616c616e63655632436f6e7472616374121e0a1541fd49eda0f23ff7ec1d03b52c3a45991c24cd440e1080ade204180170e1bbd6f3e532"` in this case.

## Use Case

Here are some use-cases for the `freezebalancev2` method:

1. **Resource Allocation for Smart Contracts**: In Web3 programming, managing resources such as energy or bandwidth is crucial for the efficient execution of smart contracts. The `freezebalancev2` method allows users to freeze a portion of their balance to obtain resources like ENERGY. This can be particularly useful for developers who need to ensure that their smart contracts have sufficient resources to execute transactions without interruption or additional costs. By freezing a balance, developers can guarantee a steady supply of necessary resources, which is essential for maintaining the reliability and performance of decentralized applications (dApps).
2. **Incentivizing Network Participation**: Another use-case for the `freezebalancev2` method is to incentivize network participation and decentralization. By freezing their balance, users can gain voting rights and participate in the governance of the blockchain network. This involvement is critical for decentralized networks that rely on community consensus to make decisions about protocol upgrades, parameter changes, or other governance matters. By encouraging users to freeze their balance and engage in governance, the network can ensure a more democratic and decentralized decision-making process.
3. **Reducing Transaction Costs**: Freezing a balance to acquire resources like ENERGY can also help users reduce transaction costs. In many blockchain networks, transaction fees are determined by the resources consumed during the execution of transactions. By using the `freezebalancev2` method to obtain these resources in advance, users can minimize the need to pay additional fees for each transaction. This can be particularly beneficial for users who frequently interact with the blockchain, as it allows them to manage their costs more effectively and make their operations more cost-efficient.

## Code for freezebalancev2

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/freezebalancev2"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "frozen_balance": 10000000,
  "resource": "ENERGY",
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

Common Errors\
When using the freezebalancev2 HTTP REST API Tron method, the following issues may occur:

* **Invalid Owner Address**: The owner\_address provided may not be valid or properly formatted. Ensure the address is a valid TRON address and is correctly encoded.
* **Insufficient Frozen Balance**: Attempting to freeze a balance greater than what is available can result in an error. Verify that the frozen\_balance does not exceed the account's available balance.
* **Incorrect Resource Type**: Specifying an unsupported resource type, such as an incorrect string for ENERGY or BANDWIDTH, will cause the method to fail. Double-check the resource parameter for accuracy.
* **Visibility Parameter Misconfiguration**: The visible parameter should be a boolean value. Ensure it is set to either true or false to prevent unexpected behavior.

The freezebalancev2 method is an essential tool in Web3 applications, enabling users to optimize their resource allocation on the TRON network. By freezing TRX, users can gain access to additional bandwidth or energy, facilitating more efficient transaction processing and enhancing decentralized application performance.

### conclusion

The provided JSON snippet illustrates a frozen balance scenario using the freezebalancev2 HTTP API on the Tron network. With an owner address of "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g" and a frozen balance of 10,000,000 in ENERGY, this API facilitates efficient resource management. By leveraging the freezebalancev2 HTTP API, users can optimize their resource allocation on the Tron blockchain.
