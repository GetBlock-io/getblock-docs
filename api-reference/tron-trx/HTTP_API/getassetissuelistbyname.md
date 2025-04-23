---
description: >-
  Retrieve asset issue details using 'getassetissuelistbyname' via Tron REST API
  Interface.
---

# getassetissuelistbyname - TRON

## Description

The 'getassetissuelistbyname' Web3 method in the Tron protocol is a powerful tool for developers seeking detailed information on asset issues by name. This REST API Interface allows users to query and retrieve a comprehensive list of asset issues, ensuring seamless integration and efficient data handling in decentralized applications. The 'getassetissuelistbyname' RPC protocol provides a robust, user-friendly platform for accessing asset data, enhancing the development and management of blockchain solutions. By leveraging this method, developers can efficiently manage and display asset information, streamline operations, and improve user experience in their decentralized applications.

## Supported Networks

The getassetissuelistbyname REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters getassetissuelistbyname method needs to be executed.

* **owner\_address** (required):
  * **Type**: String
  * **Description**: The address of the owner initiating the transaction.
  * **Format**: A valid blockchain address, typically hexadecimal.
* **contract\_address** (required):
  * **Type**: String
  * **Description**: The address of the smart contract being interacted with.
  * **Format**: A valid blockchain address, typically hexadecimal.
* **function\_selector** (required):
  * **Type**: String
  * **Description**: The function signature of the smart contract method being called.
  * **Format**: A string that specifies the function and its parameter types, e.g., "transfer(address,uint256)".
* **parameter** (required):
  * **Type**: String
  * **Description**: The encoded parameters for the function call, typically in hexadecimal format.
  * **Format**: A concatenated string of hexadecimal values representing the function's arguments.
* **fee\_limit** (required):
  * **Type**: Integer
  * **Description**: The maximum fee that the sender is willing to pay for the transaction.
  * **Units**: Typically specified in the smallest unit of the cryptocurrency (e.g., sun for TRX).
* **call\_value** (required):
  * **Type**: Integer
  * **Description**: The amount of cryptocurrency to be sent with the transaction.
  * **Units**: Typically specified in the smallest unit of the cryptocurrency (e.g., sun for TRX).
* **visible** (optional):
  * **Type**: Boolean
  * **Description**: Indicates if the transaction should be visible.
  * **Default Value**: true

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using getassetissuelistbyname

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/ \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "contract_address": "TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs",
  "function_selector": "transfer(address,uint256)",
  "parameter": "00000000000000000000004115208EF33A926919ED270E2FA61367B2DA3753DA0000000000000000000000000000000000000000000000000000000000000032",
  "fee_limit": 1000000000,
  "call_value": 0,
  "visible": true
}
```

Response

```json
{
  "Error": "class java.lang.NullPointerException : null"
}
```

## Body Parameters

Here is the list of body parameters for the `getAssetIssueListByName` method:

1. **assetName**: The name of the asset you want to retrieve the issue list for. This parameter is required to identify the specific asset.
2. **limit**: The maximum number of asset issues to return. This parameter helps to control the size of the response.
3. **offset**: The starting point for the list of asset issues. This parameter is useful for pagination, allowing you to skip a specified number of entries.
4. **sortOrder**: The order in which the asset issues should be returned, such as ascending or descending. This parameter helps in organizing the response data.
5. **includeDetails**: A boolean parameter indicating whether to include detailed information about each asset issue in the response. This can be useful if you need more than just basic information.

These parameters are used to customize the response and ensure that you receive the desired information about asset issues.

## Use Case

Here are some use-cases for the `getassetissuelistbyname` method in Web3 programming:

1. **Asset Verification and Information Retrieval**: Developers and users can utilize the `getassetissuelistbyname` method to verify the existence of a particular asset on the blockchain. By querying the blockchain for assets issued under a specific name, one can retrieve comprehensive information about the asset, including its total supply, issuer details, and issuance date. This is particularly useful for ensuring the authenticity of assets before engaging in transactions or integration into decentralized applications (dApps).
2. **Portfolio Management and Analysis**: For developers building portfolio management tools or analytics platforms, the `getassetissuelistbyname` method can be instrumental in tracking and managing digital assets. By accessing detailed information about various assets, developers can provide users with insights into their holdings, including asset performance, historical data, and potential investment opportunities. This feature enhances the user's ability to make informed decisions regarding their digital asset portfolio.
3. **Smart Contract Development and Testing**: When developing or testing smart contracts that interact with specific assets, developers can use the `getassetissuelistbyname` method to fetch necessary asset details. This ensures that the smart contract logic aligns with the asset's parameters, such as its supply and divisibility. By integrating this method into the development workflow, developers can create more robust and reliable smart contracts that handle asset interactions accurately.

## Code for getassetissuelistbyname

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
  "contract_address": "TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs",
  "function_selector": "transfer(address,uint256)",
  "parameter": "00000000000000000000004115208EF33A926919ED270E2FA61367B2DA3753DA0000000000000000000000000000000000000000000000000000000000000032",
  "fee_limit": 1000000000,
  "call_value": 0,
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

Common Errors\
When using the getassetissuelistbyname HTTP REST API Tron method, the following issues may occur:

* Invalid Asset Name: If the asset name provided does not exist or is misspelled, the API will return an empty response. Ensure the asset name is correct and matches the one registered on the Tron network.
* Network Connectivity Issues: In cases of poor network connectivity, requests to the API may time out. Verify your internet connection and try again, or consider implementing retries with exponential backoff in your application.
* API Rate Limits: Excessive requests in a short period may trigger rate limiting, resulting in temporary blocks. Monitor your request frequency and implement rate limiting strategies to stay within the allowed thresholds.
* Incorrect Parameter Formatting: If parameters are not formatted correctly, the API may not process the request. Double-check the parameter structure and ensure it adheres to the expected input format.

Using the getassetissuelistbyname method in Web3 applications provides a streamlined way to retrieve detailed information about specific assets on the Tron network. This enhances the ability to manage and interact with digital assets programmatically, offering developers the tools necessary to build robust, asset-focused decentralized applications.

### conclusion

The provided JSON represents a transaction on the Tron network, involving a transfer function. To explore detailed information about assets involved in such transactions, the getassetissuelistbyname HTTP API in Tron can be utilized. This API allows users to retrieve a list of asset issues by name, enhancing transparency and ease of access to asset data on the Tron blockchain.
