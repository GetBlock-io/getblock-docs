---
description: >-
  Use freezebalance via Tron RESTful API Interface to freeze TRX for bandwidth
  or energy, enhancing network resource management.
---

# freezebalance - TRON

## Description

The 'freezebalance' Web3 method in the Tron protocol allows users to freeze a specified amount of TRX to obtain bandwidth or energy, crucial for executing transactions and smart contracts on the network. By utilizing the 'freezebalance' RPC protocol, developers can programmatically manage network resources, ensuring efficient transaction processing and smart contract execution. This method enhances network participation by enabling users to earn voting power, which is essential for Tron’s decentralized governance. With a straightforward RESTful API Interface, developers can seamlessly integrate 'freezebalance' into their applications, optimizing resource allocation and contributing to the network's stability and efficiency.

## Supported Networks

The freezebalance REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters freezebalance method needs to be executed.

* **owner\_address** (Required)
  * Type: String
  * Description: The address of the owner whose balance is to be frozen.
  * Default/Supported Values: Must be a valid address format.
* **frozen\_balance** (Required)
  * Type: Integer
  * Description: The amount of balance to be frozen.
  * Default/Supported Values: Must be a positive integer.
* **frozen\_duration** (Required)
  * Type: Integer
  * Description: The duration for which the balance will be frozen, typically measured in days.
  * Default/Supported Values: Must be a positive integer.
* **resource** (Required)
  * Type: String
  * Description: The type of resource for which the balance is being frozen.
  * Default/Supported Values: Common values include "ENERGY".
* **visible** (Optional)
  * Type: Boolean
  * Description: Determines if the transaction is visible.
  * Default/Supported Values: true or false. Defaults to false if not specified.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using freezebalance

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/freezebalance \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "frozen_balance": 10000000,
  "frozen_duration": 3,
  "resource": "ENERGY",
  "visible": true
}
```

Response

```json
{
  "Error": "class org.tron.core.exception.ContractValidateException : freeze v2 is open, old freeze is closed"
}
```

## Body Parameters

Here is the list of body parameters for the `freezebalance` method:

1. **owner\_address**: The address of the account that owns the TRX being frozen. This should be in base58 or hex format.
2. **frozen\_balance**: The amount of TRX to freeze. This value should be specified in SUN (1 TRX = 1,000,000 SUN).
3. **frozen\_duration**: The duration for which the TRX will be frozen, usually specified in days. The minimum duration is typically 3 days.
4. **resource**: The type of resource for which the TRX is being frozen. This can be either "BANDWIDTH" or "ENERGY".
5. **receiver\_address**: (Optional) The address of the account that will receive the bandwidth or energy generated from the frozen TRX. If not specified, the resources will be credited to the owner\_address.
6. **visible**: (Optional) A boolean parameter indicating if the addresses are in base58 format. Default is false, which means addresses are in hex format.

These parameters are used to specify the details of the freezing operation in the Tron network.

## Use Case

Here are some use-cases for the freezebalance method in Web3 programming:

1. **Staking for Network Participation**: The freezebalance method can be utilized to stake tokens or resources, such as ENERGY, which allows users to participate in network governance or consensus mechanisms. By freezing a certain amount of balance, users demonstrate their commitment to the network, which may grant them voting rights or the ability to validate transactions. This is a common practice in proof-of-stake (PoS) or delegated proof-of-stake (DPoS) blockchain networks, where staking is essential for securing the network and maintaining its integrity.
2. **Resource Allocation and Management**: In blockchain ecosystems that require resource management, such as bandwidth or computing power, the freezebalance method can be used to allocate these resources effectively. By freezing a portion of their balance, users can gain access to necessary resources like ENERGY, which can be used to execute smart contracts or perform transactions without incurring additional costs. This approach helps in managing network congestion and ensures that users have a predictable and fair access to network resources.
3. **Incentivizing Long-term Holding**: Freezing balances can also serve as a mechanism to incentivize long-term holding of tokens. By offering rewards or benefits, such as increased resource allocation or reduced transaction fees, to users who freeze their balances for a specified duration, blockchain projects can encourage users to hold onto their tokens rather than selling them. This can help stabilize the token’s value and foster a more robust and committed community around the project.

## Code for freezebalance

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/freezebalance"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "frozen_balance": 10000000,
  "frozen_duration": 3,
  "resource": "ENERGY",
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

Common Errors\
When using the freezebalance HTTP REST API Tron method, the following issues may occur:

* Invalid owner address: Ensure the owner address is correctly formatted and belongs to the Tron network. Double-check for any typographical errors or missing characters.
* Insufficient balance: The specified frozen balance must not exceed the available balance in the account. Verify the account balance before initiating the freeze operation.
* Incorrect frozen duration: The frozen duration must be specified in whole days and should meet the minimum required duration set by the Tron network. Confirm that the duration is an integer and complies with network rules.
* Unsupported resource type: The resource type must be either "ENERGY" or "BANDWIDTH". Ensure that the resource parameter is correctly specified and supported by the Tron protocol.

Using the freezebalance method in Web3 applications allows developers to lock a portion of TRX tokens to gain resources like ENERGY or BANDWIDTH, which are essential for executing smart contracts and transactions without incurring additional fees. This feature enhances the efficiency and cost-effectiveness of decentralized applications on the Tron network, fostering a more seamless user experience.

### conclusion

The provided JSON snippet represents a request to the FreezeBalance HTTP API on the Tron network. This action involves freezing 10,000,000 TRX for a duration of 3 days to gain ENERGY resources. By leveraging the FreezeBalance HTTP API, users can efficiently manage their TRX resources on the Tron blockchain.
