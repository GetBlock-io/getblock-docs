---
description: >-
  Retrieve burn transaction details with the getburntrx method via Tron’s REST
  API Interface.
---

# getburntrx - TRON

## Description

The 'getburntrx' Web3 method in the Tron protocol provides a seamless way to access details of burn transactions through a RESTful API interface. This method is integral for developers needing to query and analyze burn transactions, where tokens are permanently removed from circulation, enhancing transparency and blockchain efficiency. By utilizing the 'getburntrx RPC protocol', users can efficiently retrieve transaction specifics, including transaction ID, block height, and burn amount. This method is designed to be user-friendly yet powerful, catering to both novice developers and experienced blockchain engineers looking to integrate Tron’s capabilities into their applications.

## Supported Networks

The getburntrx REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters getburntrx method needs to be executed.

* **owner\_address** (required, string): The address of the owner initiating the transaction. This is a required field and should be a valid address format.
* **to\_address** (required, string): The address to which the asset is being transferred. This is a required field and should be a valid address format.
* **asset\_name** (required, string): The name or identifier of the asset being transferred. This is a required field and should be provided in a hexadecimal string format.
* **amount** (optional, null/integer): The amount of the asset to be transferred. This field is optional, and if not provided, it may imply a default behavior or a need for clarification in the transaction process.
* **visible** (optional, boolean): A flag indicating whether the transaction should be visible. The default value is typically `false` if not specified, but in this request, it is explicitly set to `true`.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using getburntrx

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/ \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "to_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
  "asset_name": "31303030303031",
  "amount": null,
  "visible": true
}
```

Response

```json
{
  "burnTrxAmount": 14332944246680970
}
```

## Body Parameters

Here is the list of body parameters for the getburntrx method:

1. **burnTrxAmount**: This parameter indicates the total amount of TRX that has been burned. The value is typically represented as a large numerical figure, reflecting the cumulative amount over a specified period or event. In this context, the value is 14,332,944,246,680,970 TRX.

## Use Case

Here are some use-cases for the `getburntrx` method in Web3 programming:

1. **Token Burn Verification**: The `getburntrx` method can be used to verify token burn transactions on a blockchain. In scenarios where a token's supply needs to be reduced, the method helps developers confirm that a certain amount of tokens have been permanently removed from circulation. This is crucial for maintaining transparency and trust in decentralized systems, where users and stakeholders need assurance that token burns are executed as intended.
2. **Supply Management and Analytics**: Developers and analysts can use the `getburntrx` method to track and analyze the historical data of token burns. By understanding the patterns and frequency of burns, stakeholders can make informed decisions regarding token supply management, market dynamics, and future tokenomics strategies. This use-case is particularly valuable for projects looking to create deflationary mechanisms or control inflation within their ecosystem.
3. **Audit and Compliance**: The `getburntrx` method can be employed in auditing processes to ensure compliance with the project's tokenomics policies. By providing a transparent and immutable record of all burn transactions, auditors can verify that the project adheres to its stated goals and regulatory requirements. This is especially important for projects that have committed to regular token burns as part of their roadmap or whitepaper promises.

## Code for getburntrx

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
  "to_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
  "asset_name": "31303030303031",
  "amount": null,
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

Common Errors\
When using the getburntrx HTTP REST API Tron method, the following issues may occur:

* **Invalid Address Format**: If the `owner_address` or `to_address` is not in the correct format, the request will fail. Ensure that both addresses are valid Tron addresses, typically starting with "T" and followed by a series of numbers and letters.
* **Missing or Null Amount**: The `amount` field cannot be null. Specify a valid numeric value for the amount of the asset being burned to avoid request errors.
* **Incorrect Asset Name Encoding**: The `asset_name` should be correctly encoded in hexadecimal format. Double-check the asset name to ensure it is properly converted from its string representation to hexadecimal.
* **Visibility Misconfiguration**: If the `visible` parameter is incorrectly set, you might not get the expected response. Make sure the visibility setting aligns with your data requirements, either true or false.

Using the getburntrx method in Web3 applications allows developers to accurately track and verify burn transactions on the Tron blockchain, enhancing transparency and security. By leveraging this functionality, applications can offer users detailed insights into asset management and transaction history, fostering trust and engagement in decentralized ecosystems.

### conclusion

The transaction data provided, including the owner and recipient addresses, and the asset name, can be effectively managed using the getburntrx HTTP API on the Tron network. This API facilitates seamless tracking and burning of assets, ensuring transparency and efficiency in digital asset management. By leveraging the getburntrx API, users can enhance their interactions with the Tron blockchain.
