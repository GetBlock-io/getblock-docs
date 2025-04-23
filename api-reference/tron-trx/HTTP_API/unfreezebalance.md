---
description: >-
  Use the unfreezebalance method in Tron via RESTful API Interface to unlock
  your frozen TRX efficiently.
---

# unfreezebalance - TRON

## Description

The unfreezebalance Web3 method in the Tron protocol is a crucial function for users looking to regain access to their frozen TRX tokens. Through the unfreezebalance RPC protocol, this method allows users to efficiently unlock their frozen balances, enabling them to participate in transactions or other network activities without delay. Designed for seamless integration, the unfreezebalance method is accessible via a RESTful API Interface, ensuring developers can easily incorporate this functionality into their applications. By invoking this method, users can manage their assets effectively, optimizing their engagement with the Tron blockchain. Whether you're building a decentralized application or managing personal assets, the unfreezebalance method provides a reliable solution for unfreezing TRX tokens.

## Supported Networks

The unfreezebalance REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters the `unfreezebalance` method needs to be executed.

* **owner\_address** (required)
  * **Type**: String
  * **Description**: The address of the owner whose balance is being unfrozen.
  * **Default/Supported Values**: Must be a valid address format.
* **resource** (required)
  * **Type**: String
  * **Description**: The type of resource to be unfrozen.
  * **Default/Supported Values**: Supported values include "BANDWIDTH".
* **visible** (optional)
  * **Type**: Boolean
  * **Description**: Indicates whether the transaction is visible.
  * **Default/Supported Values**: Typically true or false. Default is false if not provided.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using unfreezebalance

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/ \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
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

Here is the list of body parameters for the unfreezebalance method:

1. **owner\_address**: The address of the account that owns the frozen balance. This should be a valid TRON address in base58 format.
2. **resource**: The type of resource to unfreeze. It can be either "BANDWIDTH" or "ENERGY" depending on the resource that was initially frozen.
3. **receiver\_address**: (Optional) The address of the account that will receive the resources after they are unfrozen. If not specified, the resources will be returned to the owner's address.
4. **visible**: (Optional) A boolean flag indicating whether the addresses are in base58 format (true) or hex format (false). The default value is false.
5. **timestamp**: (Optional) The timestamp of the transaction. If not specified, the current timestamp will be used.

These parameters are necessary to successfully execute the unfreezebalance method and handle the response effectively.

## Use Case

Here are some use-cases for the unfreezebalance method in Web3 programming:

1. **Restoring User Access to Resources**: In Web3 applications, particularly those built on blockchain platforms like Tron, users may have certain resources such as bandwidth or energy frozen as a part of staking or network resource allocation processes. The unfreezebalance method can be used to unfreeze these resources, effectively restoring the user's access to them. This is crucial when users need to reclaim their staked resources to perform transactions or interact with smart contracts.
2. **Facilitating Resource Management for DApps**: Decentralized applications (DApps) often require efficient resource management to ensure smooth operation. By utilizing the unfreezebalance method, developers can programmatically manage the allocation and deallocation of resources like bandwidth. This helps in optimizing the DApp's performance and ensuring that users have the necessary resources to interact with the application without interruptions.
3. **Enabling Dynamic Resource Allocation Strategies**: In blockchain ecosystems, resources are often frozen to incentivize certain behaviors, such as staking for network security. However, as network conditions change, users might need to adapt their resource allocation strategies. The unfreezebalance method allows users to dynamically adjust their resource allocations by unfreezing resources when they need to reallocate them, ensuring they can respond to changes in the network environment efficiently.

## Code for unfreezebalance

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
  "resource": "BANDWIDTH",
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

Common Errors\
When using the unfreezebalance HTTP REST API Tron method, the following issues may occur:

* **Invalid Owner Address**: If the provided `owner_address` is incorrect or not associated with any frozen balance, the request will fail. Ensure the address is correct and corresponds to an account with frozen resources.
* **Insufficient Frozen Balance**: Attempting to unfreeze more resources than available will result in an error. Verify the frozen balance of the account before making the request to ensure sufficient resources are available for unfreezing.
* **Incorrect Resource Type**: Specifying an unsupported or incorrect resource type in the `resource` field will lead to an error. Double-check the resource type, ensuring it matches the available options such as "BANDWIDTH" or "ENERGY".
* **Visibility Error**: If the `visible` parameter is set incorrectly, the response may not display the expected information. Confirm that the `visible` parameter is set to `true` to receive a response in a user-friendly format.

Using the unfreezebalance method in Web3 applications allows developers to efficiently manage and optimize resource allocation for their Tron accounts. By unfreezing resources when they are no longer needed, applications can maintain optimal performance and resource utilization, contributing to a more efficient blockchain ecosystem.

### conclusion

The unfreezebalance HTTP API in the Tron network allows users to unfreeze their resources, such as BANDWIDTH, making them available for transactions or other activities. By utilizing this API, owners like the one with the address TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g can efficiently manage their resources. This streamlined process ensures that users can optimize their participation in the Tron ecosystem without unnecessary delays.
