# createassetissue


## Meta Description
Use 'createassetissue' in Tron with the HTTP REST API Interface to issue new digital assets seamlessly.

## Description
The 'createassetissue' Web3 method in the Tron protocol allows developers to issue new digital assets on the blockchain through a user-friendly and efficient process. Utilizing the 'createassetissue' HTTP REST API protocol, this method provides a seamless interface for specifying asset details such as name, total supply, and other parameters essential for asset creation. Designed for technical users, this HTTP REST API Interface method ensures precise control over asset issuance, making it a powerful tool for developers looking to expand their blockchain projects. With detailed parameter options and robust security features, 'createassetissue' stands as a cornerstone for asset management within the Tron ecosystem, streamlining the process while maintaining high standards of reliability and performance.

## Supported Networks
The createassetissue HTTP REST API method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters the `createassetissue` method needs to be executed.

- **owner_address** (required)
  - Type: String
  - Description: The address of the owner creating the asset.
  - Default/Supported Values: Must be a valid address format, e.g., "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g".

- **name** (required)
  - Type: String (Hex-encoded)
  - Description: The name of the asset being created. It is hex-encoded.
  - Default/Supported Values: Hex-encoded string representing the asset name.

- **abbr** (required)
  - Type: String (Hex-encoded)
  - Description: The abbreviation or symbol of the asset. It is hex-encoded.
  - Default/Supported Values: Hex-encoded string representing the asset abbreviation.

- **description** (optional)
  - Type: String (Hex-encoded)
  - Description: A brief description of the asset. It is hex-encoded.
  - Default/Supported Values: Hex-encoded string, optional field.

- **url** (optional)
  - Type: String (Hex-encoded)
  - Description: A URL associated with the asset. It is hex-encoded.
  - Default/Supported Values: Hex-encoded string, optional field.

- **frozen_supply** (optional)
  - Type: Object
  - Description: Details about the frozen supply of the asset.
  - Default/Supported Values: Contains two fields:
    - **frozen_amount** (required within frozen_supply)
      - Type: Integer
      - Description: The amount of the asset that is frozen.
      - Default/Supported Values: Any non-negative integer.
    - **frozen_days** (required within frozen_supply)
      - Type: Integer
      - Description: The number of days the asset will remain frozen.
      - Default/Supported Values: Any non-negative integer.

- **visible** (optional)
  - Type: Boolean
  - Description: Indicates whether the asset is visible or not.
  - Default/Supported Values: `true` or `false`. Default is typically `false` if not specified.

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using createassetissue

Request
```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/createassetissue \
     --header 'content-type: application/json' \
     --data '
{
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "name": "0x6173736574497373756531353330383934333132313538",
  "abbr": "0x6162627231353330383934333132313538",
  "description": "0x4578616d706c654465736372697074696f6e",
  "url": "0x7777772e6578616d706c652e636f6d",
  "frozen_supply": {
    "frozen_amount": 1,
    "frozen_days": 2
  },
  "visible": true
}
'
```

Response
```json

{
  "Error": "class org.tron.core.exception.ContractValidateException : Invalid assetName"
}
```
## Body Parameters

Here is the list of body parameters for the `createassetissue` method:

1. **owner_address**: The address of the account that will own the newly created asset.
2. **name**: The name of the asset. It must be unique and follow the platform's naming rules.
3. **abbr**: The abbreviation or symbol of the asset.
4. **total_supply**: The total number of units of the asset that will be created.
5. **trx_num**: The number of TRX tokens exchanged for each unit of the asset.
6. **num**: The number of units of the asset exchanged for each TRX token.
7. **start_time**: The start time for the asset issuance.
8. **end_time**: The end time for the asset issuance.
9. **description**: A brief description of the asset.
10. **url**: A URL providing additional information about the asset.
11. **free_asset_net_limit**: The limit of free bandwidth for the asset.
12. **public_free_asset_net_limit**: The public limit of free bandwidth for the asset.
13. **frozen_supply**: Details about any frozen supply for the asset, including frozen duration and amount.

Make sure to provide valid and correctly formatted data for each parameter to avoid errors like "Invalid assetName".

## Use Case

Here are some use-cases for the `createassetissue` method in Web3 programming:

1. **Token Creation for Decentralized Applications (DApps):** The `createassetissue` method can be used to create custom tokens that serve as the native currency or utility tokens within a decentralized application. For instance, a gaming DApp might issue its own tokens to reward players for achievements or to enable in-game purchases. By creating a unique token, developers can foster an ecosystem where users engage with the application while utilizing the token for various transactions within the platform.

2. **Crowdfunding and Initial Coin Offerings (ICOs):** This method is instrumental in launching crowdfunding campaigns or initial coin offerings. By creating a new asset, startups and projects can raise funds by selling tokens to investors. These tokens often represent a stake in the project or grant access to future services. The ability to customize the token's properties, such as its supply and distribution model, allows projects to tailor their fundraising efforts to meet specific goals and attract potential investors.

3. **Community and Loyalty Programs:** Businesses can use the `createassetissue` method to develop community or loyalty tokens that reward users for their engagement and loyalty. For example, a company might issue tokens that customers can earn through purchases or participation in community activities. These tokens could then be redeemed for discounts, special offers, or exclusive access to events, thereby enhancing customer retention and fostering a sense of community around the brand.

These use cases demonstrate the versatility of the `createassetissue` method in enabling diverse token-based solutions across various sectors within the Web3 ecosystem.

## Code for createassetissue


```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/createassetissue"
data = {
    "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
    "name": "0x6173736574497373756531353330383934333132313538",
    "abbr": "0x6162627231353330383934333132313538",
    "description": "0x4578616d706c654465736372697074696f6e",
    "url": "0x7777772e6578616d706c652e636f6d",
    "frozen_supply": {
        "frozen_amount": 1,
        "frozen_days": 2
    },
    "visible": True
}

headers = {
    "Content-Type": "application/json"
}

response = requests.post(url, headers=headers, data=json.dumps(data))

if response.status_code == 200:
    print(response.json())
else:
    print(f"Error: {response.status_code}")

```
## Common Errors

Common Errors  
When using the createassetissue HTTP REST API Tron method, the following issues may occur:  
- **Invalid Address Format**: The owner_address must be a valid Tron address. Ensure the address starts with "T" and is 34 characters long to avoid this error.  
- **Hexadecimal Encoding Errors**: The name, abbreviation, description, and URL fields should be encoded in hexadecimal format. Verify that all fields are correctly encoded to prevent data misinterpretation.  
- **Frozen Supply Misconfiguration**: The frozen_amount and frozen_days should be specified correctly. Ensure that the frozen_amount is a positive integer and frozen_days is a non-negative integer to avoid incorrect asset freezing.  
- **Visibility Flag Misuse**: The visible field must be a boolean value. Double-check that this field is set to true or false to ensure proper visibility settings.

The createassetissue method is a powerful tool in Web3 applications, enabling developers to create and manage custom tokens on the Tron blockchain efficiently. By leveraging this functionality, developers can introduce new assets, facilitating innovative decentralized applications and expanding the Tron ecosystem.

### conclusion

The provided REST structure is an example of a createassetissue request in the Tron network, which is used to create a new asset. This specific request includes details such as the owner's address, asset name, abbreviation, description, and URL, as well as information about the frozen supply. By leveraging the createassetissue HTTP REST API Tron call, developers can efficiently define and issue new digital assets with customizable parameters, enhancing the versatility and functionality of the Tron blockchain ecosystem.
