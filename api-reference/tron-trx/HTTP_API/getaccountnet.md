---
description: >-
  Retrieve account bandwidth data with the 'getaccountnet' REST API Interface in
  the Tron protocol.
---

# getaccountnet - TRON

## Description

The 'getaccountnet' Web3 method in the Tron protocol is a REST API Interface designed to provide users with detailed information about the bandwidth resources of a specific account. By utilizing the 'getaccountnet' RPC protocol, developers can efficiently query and retrieve the net bandwidth consumption and remaining bandwidth of any Tron account. This method is essential for developers who need to monitor and manage account resources effectively, ensuring optimal performance and resource allocation within decentralized applications (DApps). The 'getaccountnet' function is user-friendly and integrates seamlessly into Web3 environments, making it a valuable tool for developers working with Tron’s blockchain infrastructure.

## Supported Networks

The getaccountnet REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters getaccountnet method needs to be executed.

* **address** (required)
  * **Type**: String
  * **Description**: The account address for which the net balance is to be retrieved.
  * **Default/Supported Values**: A valid address string, such as "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g".
* **visible** (optional)
  * **Type**: Boolean
  * **Description**: A flag indicating whether the response should include visible transactions.
  * **Default/Supported Values**: `true` or `false`. Default is `false` if not specified.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using getaccountnet

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/getaccountnet \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "visible": true
}
```

Response

```json

{
  "freeNetLimit": 600,
  "assetNetUsed": [
    {
      "key": "1004977",
      "value": 0
    },
    {
      "key": "1005026",
      "value": 0
    }
  ],
  "assetNetLimit": [
    {
      "key": "1004977",
      "value": 1
    },
    {
      "key": "1005026",
      "value": 0
    }
  ],
  "TotalNetLimit": 43200000000,
  "TotalNetWeight": 26514583140
}
```

## Body Parameters

Here is the list of body parameters for the `getaccountnet` method:

1. **freeNetLimit**: Represents the limit of free net usage available, set to 600.
2. **assetNetUsed**: A list of objects indicating the net usage for specific assets:
   * **key**: "1004977" with a **value** of 0.
   * **key**: "1005026" with a **value** of 0.
3. **assetNetLimit**: A list of objects representing the net limit for specific assets:
   * **key**: "1004977" with a **value** of 1.
   * **key**: "1005026" with a **value** of 0.
4. **TotalNetLimit**: The total net limit available, which is 43,200,000,000.
5. **TotalNetWeight**: The total net weight currently used, which is 26,514,583,140.

## Use Case

Here are some use-cases for the `getaccountnet` method in Web3 programming:

1. **Account Balance Verification**: One of the primary uses of the `getaccountnet` method is to verify the balance of a specific blockchain account. Developers can use this method to retrieve the net balance of an account, which is crucial for applications that need to ensure users have sufficient funds before executing transactions or smart contracts. This can help prevent failed transactions due to insufficient balance, thereby improving user experience.
2. **Transaction Monitoring and Auditing**: The `getaccountnet` method can be used to monitor and audit accounts for any changes in balance over time. By regularly checking the net balance of an account, developers can track incoming and outgoing transactions, which is essential for applications that require detailed financial reporting or fraud detection. This can be particularly useful for decentralized finance (DeFi) platforms or any application handling large volumes of transactions.
3. **Resource Management in Blockchain Applications**: In some blockchain networks, accounts may be required to hold a certain amount of resources (like bandwidth or energy) to perform transactions. The `getaccountnet` method can be used to check if an account has enough resources to execute a transaction. This ensures that applications can manage resources effectively and provide users with feedback on whether they need to acquire more resources to interact with the network.

## Code for getaccountnet

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/getaccountnet"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

Common Errors\
When using the getaccountnet HTTP REST API Tron method, the following issues may occur:

* Invalid Address Format: If the provided address does not conform to the Tron address format, the API will return an error. Ensure the address is a valid Tron address, typically starting with 'T' and followed by a string of alphanumeric characters.
* Network Connectivity Issues: If there is a disruption in network connectivity, the request may fail. Verify your network connection and ensure that the Tron node is accessible.
* Insufficient Permissions: If the API key or account does not have the necessary permissions to access the method, you will encounter an error. Check your API credentials and account settings to ensure appropriate permissions are granted.
* JSON Parsing Errors: If the request payload is not properly formatted in JSON, the server will be unable to process it. Double-check your JSON syntax to ensure it is correctly structured.

The getaccountnet method is a valuable tool in Web3 applications, allowing developers to query net bandwidth information for a specific Tron account. By leveraging this method, developers can efficiently manage and optimize resource allocation within their decentralized applications, enhancing performance and user experience.

### conclusion

The provided JSON snippet appears to be related to a Tron account, possibly obtained through the getaccountnet HTTP API. This API is a valuable tool for developers working with Tron, as it allows for easy access to account details and visibility settings. By utilizing the getaccountnet HTTP API, developers can efficiently manage and interact with Tron accounts, streamlining their blockchain development processes.
