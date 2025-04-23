---
description: 'Updateasset: REST API Interface for asset management in the Tron protocol'
---

# updateasset - TRON

## Description

The 'updateasset' Web3 method in the Tron protocol provides a powerful RESTful API Interface for modifying existing digital assets on the blockchain. Through the updateasset RPC protocol, developers can efficiently manage asset properties, such as name, description, and total supply, ensuring seamless asset lifecycle management. This method facilitates precise control and flexibility, allowing for updates while maintaining the integrity and security of the Tron network. Designed for technical users, the updateasset function requires authentication and appropriate permissions, ensuring that only authorized entities can perform updates. Its user-friendly interface and robust capabilities make it an essential tool for developers working within the Tron ecosystem.

## Supported Networks

The updateasset REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

None: This method does not require any parameters.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using updateasset

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/ \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "value": "23d11537676610c287ffcd1bc33d650df37fc90d13bb65356fbc9045cfb91705"
}
```

Response

```json

{
  "Error": "class org.tron.core.exception.ContractValidateException : Invalid ownerAddress"
}
```

## Body Parameters

Here is the list of body parameters for the `updateasset` method:

1. **ownerAddress**: The address of the owner of the asset. This parameter is crucial, and it must be valid to avoid errors such as "Invalid ownerAddress".
2. **assetName**: The name of the asset that is being updated. This should be a string representing the asset's name.
3. **totalSupply**: The total supply of the asset. This parameter specifies the total number of units available for this asset.
4. **trxNum**: The number of TRX tokens required to obtain one unit of the asset. This is used to determine the exchange rate between TRX and the asset.
5. **num**: The number of asset units that can be obtained for a specified amount of TRX.
6. **startTime**: The start time for the asset's availability. This should be a timestamp indicating when the asset will become available.
7. **endTime**: The end time for the asset's availability. This should be a timestamp indicating when the asset will no longer be available.
8. **description**: A brief description of the asset. This should provide additional information about the asset.
9. **url**: The URL associated with the asset. This could be a link to a website with more information about the asset.
10. **freeAssetNetLimit**: The maximum amount of the asset that can be transferred for free within a specified time period.
11. **publicFreeAssetNetLimit**: The maximum amount of the asset that can be transferred for free by all users within a specified time period.
12. **frozenSupply**: Information about any frozen supply of the asset, including the amount and duration of the freeze.

## Use Case

Here are some use-cases for the `updateAsset` method in Web3 programming:

1. **Asset Metadata Update**: In the context of NFTs (Non-Fungible Tokens), the `updateAsset` method can be used to modify the metadata associated with an asset. For instance, if an NFT represents a digital artwork, the artist might want to update the description, title, or even the image itself after the initial minting. This ensures that the asset remains relevant and can evolve over time, reflecting changes or improvements made by the creator.
2. **Ownership and Access Control Adjustments**: The `updateAsset` method can be utilized to change the ownership details or access permissions of a digital asset. In decentralized applications, assets may need to be transferred between users or have their access rights modified. For example, a digital asset representing a virtual real estate property could have its ownership updated when sold to another user, or its access settings adjusted to allow specific users to develop or interact with the property.
3. **Dynamic Asset Properties**: In gaming or virtual environments, assets often need to change dynamically based on user interactions or game events. The `updateAsset` method can be employed to modify properties such as the level, strength, or abilities of a character or item. This allows for a more interactive and engaging experience, as assets can evolve based on the player's actions or achievements within the game.

## Code for updateasset

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "value": "23d11537676610c287ffcd1bc33d650df37fc90d13bb65356fbc9045cfb91705"
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

Common Errors\
When using the updateasset HTTP REST API Tron method, the following issues may occur:

* **Invalid Asset ID**: The provided asset ID does not exist on the Tron network. Ensure that the asset ID is correct and corresponds to an existing asset before attempting the update.
* **Permission Denied**: The calling account does not have the necessary permissions to update the asset. Verify that the account has the appropriate rights and ownership over the asset in question.
* **Network Congestion**: High network traffic can delay or prevent the update from being processed. Consider retrying the request after some time or during off-peak hours to ensure successful execution.
* **Malformed Request**: The JSON request body is improperly formatted. Double-check the structure of your request against the API documentation to ensure all required fields and data types are correctly specified.

The updateasset method is a powerful tool in Web3 applications, allowing developers to modify existing assets on the Tron network efficiently. This functionality supports dynamic asset management, enabling seamless updates and improvements to digital assets while maintaining the security and decentralization inherent in blockchain technology.

### conclusion

The provided hash value signifies a successful transaction or operation within the Tron blockchain ecosystem. Utilizing the updateasset HTTP API in Tron allows developers to efficiently manage and modify asset details, ensuring seamless integration and up-to-date information across the network. This functionality is crucial for maintaining the integrity and reliability of asset data in decentralized applications.
