---
description: >-
  Use 'accountpermissionupdate' in Tron’s HTTP REST API Interface to manage
  account permissions efficiently.
---

# accountpermissionupdate - TRON

## Description

The accountpermissionupdate Web3 method in the Tron protocol enables users to efficiently manage and update account permissions via the HTTP REST API Interface. This REST protocol allows developers to modify the permission structure of a Tron account, providing flexibility and enhanced security for decentralized applications. By utilizing 'accountpermissionupdate', users can specify new permission settings or update existing ones, defining roles and access levels for various operations. This method is essential for maintaining control over smart contract interactions, ensuring that only authorized entities can execute specific actions. The accountpermissionupdate REST protocol is a crucial tool for developers aiming to implement robust permission management in their Tron-based applications, enhancing both security and functionality.

## Supported Networks

The accountpermissionupdate REST method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters accountpermissionupdate method needs to be executed.

* **owner\_address** (required, string): The address of the owner of the account. This is a unique identifier for the account owner.
* **actives** (required, array): A list of active permissions associated with the account.
  * **type** (required, integer): The type of the active permission. In this case, it is set to 2.
  * **permission\_name** (required, string): The name of the active permission. Here, it is "active0".
  * **threshold** (required, integer): The number of signatures required to authorize an operation. Here, it is set to 2.
  * **operations** (required, string): A hexadecimal string representing the operations that can be performed with this permission.
  * **keys** (required, array): A list of keys associated with the active permission.
    * **address** (required, string): The address associated with the key.
    * **weight** (required, integer): The weight of the key in the permission structure.
* **owner** (required, object): The owner permission structure.
  * **type** (required, integer): The type of the owner permission. In this case, it is set to 0.
  * **permission\_name** (required, string): The name of the owner permission, which is "owner".
  * **threshold** (required, integer): The number of signatures required to authorize an operation for the owner permission. Here, it is set to 1.
  * **keys** (required, array): A list of keys associated with the owner permission.
    * **address** (required, string): The address associated with the key.
    * **weight** (required, integer): The weight of the key in the permission structure.
* **visible** (optional, boolean): Indicates whether the account is visible. The default value is true.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using accountpermissionupdate

Request

```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/accountpermissionupdate \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "actives": [
    {
      "type": 2,
      "permission_name": "active0",
      "threshold": 2,
      "operations": "7fff1fc0037e0000000000000000000000000000000000000000000000000000",
      "keys": [
        {
          "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
          "weight": 1
        },
        {
          "address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
          "weight": 1
        }
      ]
    }
  ],
  "owner": {
    "type": 0,
    "permission_name": "owner",
    "threshold": 1,
    "keys": [
      {
        "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
        "weight": 1
      }
    ]
  },
  "visible": true
}
'
```

Response

```json
{
  "visible": true,
  "txID": "5914f3b4c29ab514e26be662f6028c1c5a82677acf0c88904d41a9bdd3d66877",
  "raw_data": {
    "contract": [
      {
        "parameter": {
          "value": {
            "owner": {
              "keys": [
                {
                  "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
                  "weight": 1
                }
              ],
              "threshold": 1,
              "permission_name": "owner"
            },
            "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
            "actives": [
              {
                "operations": "7fff1fc0037e0000000000000000000000000000000000000000000000000000",
                "keys": [
                  {
                    "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
                    "weight": 1
                  },
                  {
                    "address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
                    "weight": 1
                  }
                ],
                "threshold": 2,
                "type": "Active",
                "permission_name": "active0"
              }
            ]
          },
          "type_url": "type.googleapis.com/protocol.AccountPermissionUpdateContract"
        },
        "type": "AccountPermissionUpdateContract"
      }
    ],
    "ref_block_bytes": "848a",
    "ref_block_hash": "305274c7f1f088da",
    "expiration": 1745335029000,
    "timestamp": 1745334969664
  },
  "raw_data_hex": "0a02848a2208305274c7f1f088da40889accf0e5325aea01082e12e5010a3c747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e4163636f756e745065726d697373696f6e557064617465436f6e747261637412a4010a1541fd49eda0f23ff7ec1d03b52c3a45991c24cd440e12241a056f776e657220013a190a1541fd49eda0f23ff7ec1d03b52c3a45991c24cd440e1001226508021a0761637469766530200232207fff1fc0037e00000000000000000000000000000000000000000000000000003a190a1541fd49eda0f23ff7ec1d03b52c3a45991c24cd440e10013a190a154198927ffb9f554dc4a453c64b2e553a02d6df514b100170c0cac8f0e532"
}

```

## Body Parameters

Here is the list of body parameters for the account permission update method:

1. **owner\_address** (string): The address of the account whose permissions are being updated.
2. **actives** (array): A list of active permissions, each with:
   * **type** (integer): Type of permission (typically 2 for active permissions).
   * **permission\_name** (string): A name for the permission.
   * **threshold** (integer): Minimum weight required for authorization.
   * **operations** (hex string): Hexadecimal string specifying allowed operations.
   * **keys** (array): A list of key objects, each with:
     * **address** (string): The address associated with the key.
     * **weight** (integer): The weight of the key.
3. **owner** (object): The owner's permission structure, with similar fields:
   * **type** (integer)
   * **permission\_name** (string)
   * **threshold** (integer)
   * **keys** (array)
4. **witness** (object, optional): The witness permission structure (if needed).
5. **visible** (boolean): Whether the addresses are visible in base58 format (true) or hex format (false).

## Use Case

Here are some use-cases for the `accountpermissionupdate` method:

1. **Enhanced Security for Multi-Signature Wallets**: In Web3 programming, the `accountpermissionupdate` method can be used to update the permissions of a multi-signature wallet. For instance, if a new stakeholder joins a decentralized organization, the method can update the wallet's permissions to include the new member's address. This ensures that transactions require approval from the newly defined set of stakeholders, thereby enhancing the security and governance of the wallet.
2. **Dynamic Access Control**: This method can be employed to dynamically adjust access control in decentralized applications (dApps). For example, a dApp might need to alter permissions based on user roles or activities. By updating the account permissions, developers can ensure that only authorized users can perform specific operations, such as modifying smart contracts or accessing sensitive data, thus maintaining the integrity and security of the dApp.
3. **Automated Permission Adjustments**: In scenarios where permissions need to be adjusted automatically based on certain conditions or events (e.g., reaching a milestone, or a time-based trigger), the `accountpermissionupdate` method can be integrated with smart contracts to facilitate these changes without manual intervention. This can be particularly useful in decentralized finance (DeFi) applications where the conditions for transactions or user roles might evolve over time.

## Code for accountpermissionupdate

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "actives": [
    {
      "type": 2,
      "permission_name": "active0",
      "threshold": 2,
      "operations": "7fff1fc0037e0000000000000000000000000000000000000000000000000000",
      "keys": [
        {
          "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
          "weight": 1
        },
        {
          "address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
          "weight": 1
        }
      ]
    }
  ],
  "owner": {
    "type": 0,
    "permission_name": "owner",
    "threshold": 1,
    "keys": [
      {
        "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
        "weight": 1
      }
    ]
  },
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)
```

## Common Errors

**Common Errors**\
When using the accountpermissionupdate HTTP REST API Tron method, the following issues may occur:

* **Incorrect Threshold Configuration**: If the threshold value is set higher than the total weight of the keys, the account will be unable to authorize any transactions. Ensure the threshold does not exceed the sum of all key weights.
* **Invalid Operations Hex String**: The operations field must be a valid hexadecimal string representing the operations the permission can execute. Verify that the string is correctly formatted and corresponds to the intended operations.
* **Key Weight Mismatch**: If the weights of the keys do not align with the required threshold, transactions may fail. Double-check that the total weight of the keys meets or exceeds the threshold value to ensure successful authorization.
* **Address Format Errors**: Addresses must be in the correct Tron address format. Verify each address for typographical errors or incorrect formatting to prevent authorization failures.

Using the accountpermissionupdate method in Web3 applications allows for dynamic management of account permissions, enhancing security and flexibility. By updating permissions programmatically, developers can create more robust and adaptable decentralized applications that cater to evolving user requirements and security policies.

### conclusion

The accountpermissionupdate operation outlined in the provided HTTP REST API structure demonstrates how to configure permissions for a Tron account using the accountpermissionupdate HTTP REST API. By setting thresholds and assigning weights to different addresses, the account can ensure that certain operations require multiple approvals, enhancing security. This setup is crucial for managing permissions effectively within the Tron blockchain ecosystem.
