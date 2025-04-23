---
description: >-
  Access getapprovedlist using the Tron REST API Interface for streamlined Web3
  interactions.
---

# getapprovedlist - TRON

## Description

The getapprovedlist method in the Tron protocol provides developers with a powerful tool to retrieve a list of addresses that have been approved for specific transactions or contracts. By leveraging the 'getapprovedlist Web3' functionality, developers can seamlessly integrate this method into their decentralized applications, ensuring efficient and secure access to approved addresses. The 'getapprovedlist RPC protocol' facilitates direct communication between clients and the Tron blockchain, providing a reliable and efficient means of data retrieval. This method is essential for developers seeking to enhance their applications with precise access control and streamlined Web3 interactions, ensuring both security and performance in their blockchain solutions.

## Supported Networks

The getapprovedlist REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters getapprovedlist method needs to be executed.

* **to\_address** (required)
  * **Type**: String
  * **Description**: The recipient address for the transaction.
  * **Supported Values**: Must be a valid blockchain address format, typically a base58 encoded string.
* **owner\_address** (required)
  * **Type**: String
  * **Description**: The address of the owner initiating the transaction.
  * **Supported Values**: Must be a valid blockchain address format, typically a base58 encoded string.
* **asset\_name** (required)
  * **Type**: String
  * **Description**: The identifier for the asset involved in the transaction.
  * **Supported Values**: Typically a hexadecimal string representing the asset.
* **visible** (optional)
  * **Type**: Boolean
  * **Description**: Indicates whether the transaction details should be visible.
  * **Default Value**: `true`
  * **Supported Values**: `true` or `false`

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using getapprovedlist

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/getapprovedlist \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "signature": [
    "e0bd4a60f1b3c89d4da3894d400e7e32385f6dd690aee17fdac4e016cdb294c5128b66f62f3947a7182c015547496eba95510c113bda2a361d811b829343c36501",
    "596ead6439d0f381e67f30b1ed6b3687f2bd53ce5140cdb126cfe4183235804741eeaf79b4e91f251fd7042380a9485d4d29d67f112d5387bc7457b355cd3c4200"
  ],
  "raw_data": {
    "contract": [
      {
        "parameter": {
          "value": {
            "amount": 1000000,
            "owner_address": "41A7D8A35B260395C14AA456297662092BA3B76FC0",
            "to_address": "415A523B449890854C8FC460AB602DF9F31FE4293F"
          },
          "type_url": "type.googleapis.com/protocol.TransferContract"
        },
        "type": "TransferContract"
      }
    ],
    "ref_block_bytes": "163d",
    "ref_block_hash": "77ef4ace148b05ba",
    "expiration": 1555664823000,
    "timestamp": 1555664763418
  }
}
'
```

Response

```json
{
  "result": {},
  "approved_list": [
    "41d795dc6b63cccd17ef7a7cc22116d18089f5acbb",
    "41ac1e00f5a45c769b7da5e4148c129ee716e4973e"
  ],
  "transaction": {
    "result": {
      "result": true
    },
    "txid": "120c37304bef81be37461ad3fc6bc7f5105db2d1d701c54f95093943dac607de",
    "transaction": {
      "signature": [
        "e0bd4a60f1b3c89d4da3894d400e7e32385f6dd690aee17fdac4e016cdb294c5128b66f62f3947a7182c015547496eba95510c113bda2a361d811b829343c36501",
        "596ead6439d0f381e67f30b1ed6b3687f2bd53ce5140cdb126cfe4183235804741eeaf79b4e91f251fd7042380a9485d4d29d67f112d5387bc7457b355cd3c4200"
      ],
      "txID": "120c37304bef81be37461ad3fc6bc7f5105db2d1d701c54f95093943dac607de",
      "raw_data": {
        "contract": [
          {
            "parameter": {
              "value": {
                "amount": 1000000,
                "owner_address": "41a7d8a35b260395c14aa456297662092ba3b76fc0",
                "to_address": "415a523b449890854c8fc460ab602df9f31fe4293f"
              },
              "type_url": "type.googleapis.com/protocol.TransferContract"
            },
            "type": "TransferContract"
          }
        ],
        "ref_block_bytes": "163d",
        "ref_block_hash": "77ef4ace148b05ba",
        "expiration": 1555664823000,
        "timestamp": 1555664763418
      },
      "raw_data_hex": "0a02163d220877ef4ace148b05ba40d8c5e5a6a32d5a67080112630a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412320a1541a7d8a35b260395c14aa456297662092ba3b76fc01215415a523b449890854c8fc460ab602df9f31fe4293f18c0843d709af4e1a6a32d"
    }
  }
}
```

## Body Parameters

Here is the list of body parameters for the `getapprovedlist` method:

1. **status**: The current status of the approval list. Typically includes values like "approved", "pending", or "rejected".
2. **requestId**: A unique identifier for the approval request. This helps in tracking and managing individual requests.
3. **requesterName**: The name of the person or entity that submitted the approval request.
4. **approvalDate**: The date when the request was approved. This is usually in a standard date format (e.g., YYYY-MM-DD).
5. **approverId**: The identifier of the person or system that approved the request. This helps in auditing and tracking who approved what.
6. **comments**: Any additional comments or notes associated with the approval request. This may include reasons for approval or other relevant information.
7. **priority**: The priority level of the request, which might affect how quickly it is processed. Common values could be "high", "medium", or "low".
8. **attachmentUrls**: A list of URLs pointing to any documents or files attached to the approval request.
9. **department**: The department or division within the organization that is associated with the approval request.
10. **expiryDate**: The date when the approval is no longer valid or needs to be reviewed again.

These parameters are typically included in the response body of the `getapprovedlist` method to provide detailed information about each approved request.

## Use Case

Here are some use-cases for the `getapprovedlist` method in Web3 programming:

1. **Asset Management and Verification**: The `getapprovedlist` method can be utilized to manage and verify the list of approved addresses for a specific asset. In decentralized applications (dApps), ensuring that only authorized users can interact with certain assets is crucial for maintaining security and integrity. By retrieving the list of approved addresses, developers can implement checks to validate transactions and operations involving the asset, ensuring that only those with the necessary permissions can execute certain actions.
2. **Access Control in Decentralized Applications**: In scenarios where access to certain functionalities or data within a dApp is restricted, the `getapprovedlist` method can be employed to enforce access control. By maintaining a list of approved addresses, dApps can restrict functionalities such as voting, content creation, or exclusive feature access to a specific group of users. This ensures that only users who have been granted permission can utilize these features, enhancing the security and exclusivity of the application.
3. **Automated Compliance and Auditing**: For projects that need to adhere to regulatory requirements or internal policies, the `getapprovedlist` method can facilitate automated compliance checks and auditing processes. By regularly retrieving and reviewing the list of approved addresses, organizations can ensure that their operations remain compliant with the necessary regulations. This can be particularly useful for financial services, tokenized assets, and other sectors where compliance is critical.

## Code for getapprovedlist

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "to_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "asset_name": "1000001031303030303031",
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

Common Errors\
When using the getapprovedlist HTTP REST API Tron method, the following issues may occur:

* **Invalid Address Format**: If the `to_address` or `owner_address` is not in the correct format, the request will fail. Ensure that both addresses follow the Tron address format, typically starting with a 'T' and consisting of 34 characters.
* **Incorrect Asset Name Encoding**: The `asset_name` should be encoded in hexadecimal format. Double-check your asset name encoding to prevent mismatches that could lead to errors.
* **Visibility Parameter Misuse**: The `visible` parameter must be a boolean value (`true` or `false`). Using any other type will result in an error, so ensure this parameter is correctly set.
* **Unauthorized Access**: If the `owner_address` does not have the necessary permissions to access the approved list, the request will be denied. Verify that the address has the appropriate rights or use an address with sufficient privileges.

Utilizing the getapprovedlist method in Web3 applications provides a streamlined way to verify and manage approved asset transfers, enhancing security and operational efficiency. This functionality is vital for maintaining transparency and trust in decentralized applications, ensuring that only authorized transactions are processed.

### conclusion

The provided JSON snippet seems to be a transaction request on the Tron network, involving a transfer of a specific asset. To manage and verify such transactions efficiently, developers can utilize the getapprovedlist HTTP API on Tron. This API allows users to access a list of approved transactions, ensuring transparency and security in asset transfers. By integrating the getapprovedlist HTTP API into applications, developers can enhance transaction oversight and streamline operations within the Tron ecosystem.
