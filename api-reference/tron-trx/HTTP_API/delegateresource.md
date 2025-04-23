---
description: >-
  Use the delegateresource method in the Tron protocol's HTTP REST API Interface
  for efficient resource delegation.
---

# delegateresource - TRON

## Description

The delegateresource Web3 method in the Tron protocol enables users to delegate resources such as bandwidth or energy to other accounts, enhancing their ability to perform transactions or execute smart contracts. This function is part of the 'delegateresource HTTP REST protocol', allowing for seamless resource management within the Tron network. By leveraging this method, users can optimize resource allocation, ensuring efficient network operations and cost-effective transactions. The 'delegateresource' method is accessible through the HTTP REST API Interface, providing developers with a robust tool for integrating resource delegation capabilities into their applications. This feature is essential for developers seeking to build scalable and efficient decentralized applications on the Tron blockchain.

## Supported Networks

The delegateresource HTTP REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters delegateresource method needs to be executed.

* **owner\_address** (required)
  * Type: String
  * Description: The address of the owner who is delegating the resource.
  * Supported Values: A valid blockchain address.
* **receiver\_address** (required)
  * Type: String
  * Description: The address of the receiver who will receive the delegated resource.
  * Supported Values: A valid blockchain address.
* **balance** (required)
  * Type: Integer
  * Description: The amount of resource to be delegated.
  * Supported Values: A positive integer representing the resource amount.
* **resource** (required)
  * Type: String
  * Description: The type of resource being delegated.
  * Supported Values: "BANDWIDTH"
* **lock** (optional)
  * Type: Boolean
  * Description: Indicates whether the resource should be locked during delegation.
  * Default Value: false
* **visible** (optional)
  * Type: Boolean
  * Description: Determines if the transaction should be visible in a readable format.
  * Default Value: true

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using delegateresource

Request

```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/delegateresource \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "receiver_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
  "balance": 1000000,
  "resource": "BANDWIDTH",
  "lock": false,
  "visible": true
}
'
```

Response

```json
{
  "Error": "class org.tron.core.exception.ContractValidateException : delegateBalance must be less than or equal to available FreezeBandwidthV2 balance"
}
```

## Body Parameters

Here is the list of body parameters for the `delegateresource` method:

| Parameter             | Type      | Description                                                                                                                                                                                                                                         |
| --------------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **owner\_address**    | `string`  | The address delegating resources (owner). Default: `TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g` (hexString).                                                                                                                                                |
| **receiver\_address** | `string`  | The address receiving delegated resources. Default: `TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1`.                                                                                                                                                           |
| **balance**           | `int64`   | Amount of TRX staked (in **sun**). Default: `1000000`.                                                                                                                                                                                              |
| **resource**          | `string`  | Resource type: `"BANDWIDTH"` or `"ENERGY"`. Default: `"BANDWIDTH"`.                                                                                                                                                                                 |
| **lock**              | `boolean` | If `true`, resources are locked until `lock_period` ends. Default: `false`.                                                                                                                                                                         |
| **lock\_period**      | `int64`   | <p>Lock duration in <strong>block intervals</strong> (1 block = 3 sec).<br>• Min: Remaining lock period (if reapplied).<br>• Max: <code>864000</code> (30 days).<br>• Default: <code>86400</code> (3 days) if <code>lock=true</code> and unset.</p> |
| **visible**           | `boolean` | Whether addresses are in base58 format. Default: `true`.                                                                                                                                                                                            |
| **permission\_id**    | `int32`   | _(Optional)_ Multi-signature permission ID.                                                                                                                                                                                                         |

These parameters must be carefully set to ensure that the delegation transaction is valid and does not exceed any available balances or restrictions.

## Use Case

Here are some use-cases for the `delegateresource` method:

1. **Optimizing Resource Utilization**: In Web3 environments, resources such as bandwidth or energy are often limited and need to be managed efficiently. The `delegateresource` method allows users to delegate their excess resources to other users or applications. For instance, if a user has more bandwidth than they need, they can delegate it to a smart contract or another user who requires additional bandwidth. This not only optimizes resource utilization but also enhances the overall network efficiency by ensuring that resources are not wasted.
2. **Facilitating DApp Operations**: Decentralized applications (DApps) often require consistent access to network resources to function smoothly. By using the `delegateresource` method, DApp developers or operators can ensure that their applications have the necessary resources to handle user interactions without interruptions. This is particularly useful during peak times or when launching new features that might temporarily increase resource demands.
3. **Supporting New Users**: In many blockchain networks, new users may find it challenging to acquire the necessary resources to begin interacting with the network. By delegating resources to these new users, existing members or community leaders can help onboard them more smoothly. This can be part of community-building efforts, encouraging adoption, and reducing barriers to entry for newcomers who might otherwise be deterred by the initial resource requirements.

## Code for delegateresource

```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/delegateresource"

headers = {
    "accept": "application/json",
    "content-type": "application/json"
}

payload = {
    "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
    "receiver_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
    "balance": 1000000,
    "resource": "BANDWIDTH",
    "lock": False,
    "visible": True
}

response = requests.post(url, json=payload, headers=headers)

print(response.status_code)
print(response.json())
```

## Common Errors

Common Errors\
When using the delegateresource HTTP REST API Tron method, the following issues may occur:

* Invalid Owner or Receiver Address: Ensure that both the owner and receiver addresses are correctly formatted and belong to valid Tron accounts. Double-check for any typographical errors or missing characters.
* Insufficient Balance: The owner account must have enough TRX to cover the transaction fees and the specified balance to delegate. Verify the account balance and adjust the amount accordingly.
* Resource Type Mismatch: The specified resource type must be either "BANDWIDTH" or "ENERGY." Confirm the resource type is correctly specified to avoid processing errors.
* Lock Status Error: If the lock parameter is incorrectly set or conflicts with the account's current state, ensure it reflects the intended operation, and the account settings allow for resource delegation.

The delegateresource method in Web3 applications provides a streamlined way to manage resource allocation between Tron accounts, enhancing transaction efficiency and network participation. By delegating resources like bandwidth or energy, users can optimize their network usage and support other accounts, fostering a more collaborative and resource-efficient blockchain ecosystem.

### conclusion

The provided HTTP REST API object represents a transaction on the Tron network where bandwidth resources are being delegated from the owner to the receiver. This transaction can be executed using the `delegateresource` HTTP REST API method, which facilitates the delegation of resources such as bandwidth or energy on the Tron blockchain. By utilizing the `delegateresource` HTTP Tron method, users can efficiently manage their resource allocation and optimize network performance for their transactions.
