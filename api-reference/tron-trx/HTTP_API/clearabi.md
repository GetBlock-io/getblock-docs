---
description: >-
  The 'clearabi' method in Tron’s HTTP REST API Interface removes contract ABI
  data efficiently.
---

# clearabi - TRON

## Description

The 'clearabi' Web3 method within the Tron protocol's HTTP REST API protocol is designed to efficiently remove the Application Binary Interface (ABI) data associated with a smart contract. This method is a part of the HTTP REST API Interface, allowing developers to manage smart contract data effectively. By executing 'clearabi', users can clear outdated or unnecessary ABI information, ensuring that the contract's storage remains optimized and up-to-date. This function is crucial for developers looking to maintain clean and efficient contract management on the Tron network. The 'clearabi' HTTP protocol is a technical tool that aids in the seamless operation and maintenance of smart contracts by facilitating the removal of ABI data when it's no longer needed.

## Supported Networks

The clearabi HTTP REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters clearabi method needs to be executed.

* **owner\_address** (required):
  * **Type**: String
  * **Description**: The address of the owner initiating the request.
  * **Default/Supported Values**: Must be a valid address format, such as "TSNEe5Tf4rnc9zPMNXfaTF5fZfHDDH8oyW".
* **contract\_address** (required):
  * **Type**: String
  * **Description**: The address of the contract for which the ABI is to be cleared.
  * **Default/Supported Values**: Must be a valid contract address format, such as "TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs".
* **visible** (optional):
  * **Type**: Boolean
  * **Description**: Indicates whether the transaction should be visible.
  * **Default/Supported Values**: `true` or `false`. Default is `true`.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using clearabi

Request

```json
curl --request POST \
     --url https://api.shasta.trongrid.io/wallet/clearabi \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "owner_address": "TSNEe5Tf4rnc9zPMNXfaTF5fZfHDDH8oyW",
  "contract_address": "TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs",
  "visible": true
}
'
```

Response

```json
{
  "visible": true,
  "txID": "cae0c12c78bcaecb5b45c82c9ec686c15a601e82d9c54b13cf726774009b7dbf",
  "raw_data": {
    "contract": [
      {
        "parameter": {
          "value": {
            "owner_address": "TSNEe5Tf4rnc9zPMNXfaTF5fZfHDDH8oyW",
            "contract_address": "TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs"
          },
          "type_url": "type.googleapis.com/protocol.ClearABIContract"
        },
        "type": "ClearABIContract"
      }
    ],
    "ref_block_bytes": "86ca",
    "ref_block_hash": "88e2cf7cf76f1910",
    "expiration": 1745336775000,
    "timestamp": 1745336716615
  },
  "raw_data_hex": "0a0286ca220888e2cf7cf76f191040d8e2b6f1e5325a630830125f0a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e436c656172414249436f6e7472616374122e0a1541b3dcf27c251da9363f1a4888257c16676cf54edf12154142a1e39aefa49290f2b3f9ed688d7cecf86cd6e070c79ab3f1e532"
}
```

## Body Parameters

Here is the list of body parameters for the clearabi method:

* **owner\_address**
  * **Type:** `string`
  * **Default:** `TSNEe5Tf4rnc9zPMNXfaTF5fZfHDDH8oyW`
  * **Description:** Owner address of the smart contract. If `visible=true`, use base58check format, otherwise use hex format.
* **contract\_address**
  * **Type:** `string`
  * **Default:** `TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs`
  * **Description:** Smart contract address. If `visible=true`, use base58check format, otherwise use hex format.
* **visible**
  * **Type:** `boolean`
  * **Default:** `true`
  * **Description:** Optional. Defaults to `false`. Whether addresses are in base58check format.

## Use Case

Here are some use-cases for the `clearabi` method in Web3 programming:

1. **Contract Upgrade and Maintenance**: In the rapidly evolving blockchain ecosystem, smart contracts may require upgrades or modifications to improve functionality or security. The `clearabi` method can be used to remove the existing Application Binary Interface (ABI) from a contract, which is a crucial step when preparing to update or redeploy a contract. By clearing the ABI, developers can ensure that any outdated or vulnerable interfaces are removed, minimizing potential risks and ensuring that the contract operates with the most current and secure code.
2. **Privacy and Security**: In certain scenarios, maintaining privacy and security is essential for smart contract operations. By using the `clearabi` method, developers can prevent unauthorized access to the contract's functions by removing the ABI, which serves as a map for interacting with the contract. This can be particularly useful in cases where the contract is no longer intended for public interaction, or when sensitive operations need to be shielded from public view.
3. **Decommissioning Contracts**: When a smart contract has fulfilled its purpose or is no longer needed, developers may choose to decommission it. The `clearabi` method can be used as part of the decommissioning process to remove the ABI, effectively disabling any further interaction with the contract. This helps to ensure that the contract is not mistakenly used or referenced in the future, thereby reducing clutter and potential confusion in the blockchain ecosystem.

## Code for clearabi

```python
import requests
import json

url = "https://api.shasta.trongrid.io/wallet/clearabi"
data = {
    "owner_address": "TSNEe5Tf4rnc9zPMNXfaTF5fZfHDDH8oyW",
    "contract_address": "TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs",
    "visible": True
}

headers = {
    "Accept": "application/json",
    "Content-Type": "application/json"
}

response = requests.post(url, headers=headers, data=json.dumps(data))

if response.status_code == 200:
    print(response.json())
else:
    print(f"Error: {response.status_code}")

```

## Common Errors

Common Errors\
When using the clearabi HTTP Tron method, the following issues may occur:

* Invalid owner address: Ensure the provided owner address is correct and associated with your Tron account. Double-check for any typographical errors or missing characters.
* Unauthorized access: The caller must have the necessary permissions to clear the ABI of the contract. Verify that the correct private key is being used and that it has the appropriate permissions.
* Contract address mismatch: The specified contract address must exist on the Tron network and correspond to a deployed contract. Confirm that the contract address is accurate and deployed on the correct network.
* Network connectivity issues: Ensure you have a stable internet connection and that your node is properly synchronized with the Tron network. Consider checking your node configuration for any discrepancies.

Using the clearabi method in Web3 applications allows developers to remove the ABI from a contract, which can be useful for privacy or security reasons. By clearing the ABI, you can prevent unauthorized interactions with the contract's functions, thus enhancing the security of your decentralized applications.

### conclusion

The provided REST snippet highlights crucial details about a Tron-based smart contract, including the owner and contract addresses. Utilizing ClearABI with Tron and REST can significantly streamline the interaction process with such contracts, ensuring seamless and efficient blockchain operations. By leveraging ClearABI, developers can enhance their ability to manage and execute smart contracts on the Tron network effectively.
