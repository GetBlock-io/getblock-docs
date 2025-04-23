---
description: >-
  unfreezebalancev2 RESTful API Interface for Tron Protocol, unlocking frozen
  assets seamlessly.
---

# unfreezebalancev2 - TRON

## Description

The 'unfreezebalancev2' Web3 method is an integral part of the Tron protocol, designed to allow users to unlock their frozen assets efficiently. This method is accessible via the RESTful API Interface, providing a streamlined approach to interact with the Tron blockchain. Utilizing the 'unfreezebalancev2' RPC protocol, developers can programmatically unfreeze balances, enabling a smooth transition of assets back into circulation. This function is essential for participants seeking to regain control over their previously frozen TRX or other tokens, ensuring liquidity and flexibility within the Tron ecosystem. By leveraging this API method, users can enhance their blockchain interactions, making asset management more intuitive and efficient.

## Supported Networks

The unfreezebalancev2 REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters unfreezebalancev2 method needs to be executed.

* **owner\_address** (required)
  * **Type**: String
  * **Description**: The address of the account owner who wishes to unfreeze their balance.
  * **Default/Supported Values**: A valid blockchain address, e.g., "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g".
* **unfreeze\_balance** (required)
  * **Type**: Integer
  * **Description**: The amount of balance to be unfrozen, specified in the smallest unit of the currency.
  * **Default/Supported Values**: A positive integer, e.g., 1000000.
* **resource** (required)
  * **Type**: String
  * **Description**: The type of resource for which the balance is being unfrozen.
  * **Default/Supported Values**: "BANDWIDTH" (other resources may be supported depending on the system).
* **visible** (optional)
  * **Type**: Boolean
  * **Description**: Indicates whether the transaction should be visible in the blockchain explorer.
  * **Default/Supported Values**: true or false, default is true if not specified.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using unfreezebalancev2

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/ \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "unfreeze_balance": 1000000,
  "resource": "BANDWIDTH",
  "visible": true
}
```

Response

```json

{
  "Error": "class org.tron.core.exception.ContractValidateException : no frozenBalance(BANDWIDTH)"
}
```

## Body Parameters

Here is the list of body parameters for the unfreezebalancev2 method:

1. **owner\_address**: The address of the account that owns the frozen balance. This field is required to identify which account's balance is being unfrozen.
2. **resource**: Indicates the type of resource for which the balance was frozen. It can typically be either "BANDWIDTH" or "ENERGY". In this context, the error message suggests an issue with the "BANDWIDTH" resource.
3. **receiver\_address**: (Optional) The address of the account that will receive the resources after they are unfrozen. If not specified, the resources will be returned to the owner address.
4. **timestamp**: The time at which the unfreeze request is made. This helps in ensuring the request is processed in a timely manner.
5. **signature**: A digital signature created by the owner of the account. This is used to verify the authenticity of the request and ensure that it is authorized by the account owner.
6. **data**: (Optional) Any additional data or metadata that might be necessary for the unfreeze operation or for tracking purposes.

These parameters are essential for processing the unfreezebalancev2 request, ensuring that the correct balance is unfrozen and returned to the appropriate account.

## Use Case

Here are some use-cases for the `unfreezebalancev2` method:

1. **Resource Management in DApps**: In decentralized applications (DApps), efficient resource management is crucial for maintaining performance and minimizing costs. The `unfreezebalancev2` method can be used to unfreeze a specified amount of resources, such as bandwidth, allowing developers to allocate these resources to critical operations or users who need them the most. This flexibility helps in optimizing the use of available resources, ensuring that the DApp remains responsive and efficient even during peak usage times.
2. **User Incentives and Rewards**: In Web3 ecosystems, incentivizing user participation is a common practice. By utilizing the `unfreezebalancev2` method, developers can unfreeze resources that were previously locked as part of a reward mechanism. This can be used to release bandwidth or other resources back to users as a reward for their contributions, such as participating in governance, providing liquidity, or engaging with the platform. This approach not only encourages user engagement but also fosters a sense of ownership and active participation in the ecosystem.
3. **Resource Optimization for Smart Contracts**: Smart contracts often require specific resources to execute transactions or perform computations. The `unfreezebalancev2` method allows developers to dynamically manage and optimize these resources by unfreezing them when needed. This is particularly useful in scenarios where a smart contract needs additional bandwidth to handle a sudden increase in transaction volume. By strategically unfreezing resources, developers can ensure that their smart contracts continue to function smoothly without interruptions or delays, thereby enhancing the user experience and reliability of the platform.

## Code for unfreezebalancev2

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
  "unfreeze_balance": 1000000,
  "resource": "BANDWIDTH",
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

Common Errors\
When using the unfreezebalancev2 HTTP REST API Tron method, the following issues may occur:

* Invalid owner\_address: Ensure that the owner\_address is a valid Tron address. Double-check the address format and length to prevent this error.
* Insufficient unfreeze\_balance: The specified unfreeze\_balance must not exceed the available frozen balance. Verify the current frozen balance before making the request.
* Incorrect resource type: The resource parameter must be either "BANDWIDTH" or "ENERGY". Ensure you are using the correct resource type to avoid this error.
* Visibility flag issue: The visible parameter should be a boolean value. Confirm that it is set to either true or false to ensure proper API request processing.

The unfreezebalancev2 method is a powerful tool in Web3 applications, providing users the ability to manage their TRX resources effectively. By leveraging this method, developers can create more dynamic and resource-efficient applications, enhancing user experience and interaction within the Tron ecosystem.

### conclusion

The provided JSON snippet appears to be a response from the unfreezebalancev2 HTTP API on the Tron network. This API allows users to unfreeze a specified amount of resources, such as BANDWIDTH, from a given address. In this case, 1,000,000 units of BANDWIDTH are being unfrozen for the owner address "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g". The unfreezebalancev2 API is an essential tool for managing resource allocations on the Tron blockchain, providing flexibility and control over resource usage.
