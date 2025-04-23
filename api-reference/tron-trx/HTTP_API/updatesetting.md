# updatesetting


## Meta Description
'updatesetting' RESTful API Interface for Tron protocol configuration.

## Description
The 'updatesetting' Web3 method in the Tron protocol provides a streamlined approach to configuring network settings via a RESTful API Interface. This method allows developers to modify specific parameters within the Tron network, ensuring optimal performance and customization. Utilizing the 'updatesetting RPC protocol', users can execute precise adjustments, enhancing the network's adaptability to evolving requirements. Designed for technical efficiency, this method supports seamless integration with existing systems, empowering developers to implement changes without disrupting ongoing operations. The user-friendly interface simplifies complex configurations, making it an essential tool for maintaining and optimizing Tron network settings.

## Supported Networks
The updatesetting REST API method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters updatesetting method needs to be executed.

1. **owner_address**  
   - *Type*: `string`  
   - *Description*: The account address initiating the transaction  
   - *Format*: Base58 or hex string  
   - *Example*:  
     ```
     TSNEe5Tf4rnc9zPMNXfaTF5fZfHDDH8oyW
     ```

2. **contract_address**  
   - *Type*: `string`  
   - *Description*: The smart contract address being interacted with  
   - *Format*: Base58 or hex string  
   - *Example*:  
     ```
     TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs
     ```

3. **consume_user_resource_percent**  
   - *Type*: `int64`  
   - *Range*: 0-100  
   - *Description*: Percentage of energy cost allocated to user (remainder paid by contract)  
   - *Special Values*:  
     - `0`: Contract pays 100% of energy costs  
     - `100`: User pays 100% of energy costs  
   - *Default*: `10`  
   - *Example*:  
     ```json
     25
     ```

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using updatesetting

Request
```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/updatesetting \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "owner_address": "TSNEe5Tf4rnc9zPMNXfaTF5fZfHDDH8oyW",
  "contract_address": "TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs",
  "consume_user_resource_percent": 10,
  "visible": true
}
'
```

Response
```json
{
  "visible": true,
  "txID": "b5d0df71bf1450029f818eccb24f58e356dc66984ad9ca7faaf8aec4bdc627f1",
  "raw_data": {
    "contract": [
      {
        "parameter": {
          "value": {
            "consume_user_resource_percent": 10,
            "owner_address": "TSNEe5Tf4rnc9zPMNXfaTF5fZfHDDH8oyW",
            "contract_address": "TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs"
          },
          "type_url": "type.googleapis.com/protocol.UpdateSettingContract"
        },
        "type": "UpdateSettingContract"
      }
    ],
    "ref_block_bytes": "9a42",
    "ref_block_hash": "ac02162f0c35ed19",
    "expiration": 1745351877000,
    "timestamp": 1745351819731
  },
  "raw_data_hex": "0a029a422208ac02162f0c35ed194088c3d0f8e5325a6a082112660a32747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e55706461746553657474696e67436f6e747261637412300a1541b3dcf27c251da9363f1a4888257c16676cf54edf12154142a1e39aefa49290f2b3f9ed688d7cecf86cd6e0180a70d383cdf8e532"
}
```
## Body Parameters

Here is the list of body parameters for the `updatesetting` method:

1. **owner_address**:  
   - *Type*: `string` (hex format)  
   - *Required*: Yes  
   - *Default*: `TSNEe5Tf4rnc9zPMNXfaTF5fZfHDDH8oyW`  
   - *Description*: The transaction creator's address in hexadecimal string format.  
   - *Example*:  
     ```
     TSNEe5Tf4rnc9zPMNXfaTF5fZfHDDH8oyW
     ```

2. **contract_address**:  
   - *Type*: `string` (hex format)  
   - *Required*: Yes  
   - *Default*: `TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs`  
   - *Description*: The address of the contract being modified in hexadecimal format.  
   - *Example*:  
     ```
     TG3XXyExBkPp9nzdajDZsozEu4BkaSJozs
     ```

3. **consume_user_resource_percent**:  
   - *Type*: `int32`  
   - *Required*: Yes  
   - *Default*: `10`  
   - *Range*: 0-100  
   - *Description*: Percentage of user's resources to consume.  
     - `0`: Only consumes developer resources  
     - `100`: Fully consumes user resources  
   - *Example*:  
     ```json
     10
     ```

4. **Permission_id**:  
   - *Type*: `int32`  
   - *Required*: No  
   - *Description*: Used for multi-signature transactions when applicable.

5. **visible**:  
   - *Type*: `boolean`  
   - *Default*: `true`  
   - *Description*: Controls address display format:  
     - `true`: Base58 format (human-readable)  
     - `false`: Raw hexadecimal format  
   - *Example Values*:  
     ```json
     true
     false
     ```

## Use Case

Here are some use-cases for the `updatesetting` method in Web3 programming:

1. **Dynamic Adjustment of Fees**: In decentralized applications (dApps) that involve transactions or exchanges, the `updatesetting` method can be used to dynamically adjust brokerage or transaction fees. For instance, if the market conditions change or if there's a need to incentivize users, the brokerage fee can be updated from 20% to a different value, ensuring the platform remains competitive and attractive to users.

2. **Visibility Control for Smart Contracts**: The `updatesetting` method can manage the visibility of certain features or contracts within a dApp. By toggling the `visible` property, developers can control which parts of the application are accessible to users at any given time. This can be particularly useful during maintenance, updates, or when rolling out new features incrementally to test user response.

3. **Ownership and Access Management**: The `updatesetting` method can be employed to update the `owner_address` of a smart contract or dApp component. This is crucial in scenarios where the ownership needs to be transferred due to changes in management or partnerships, ensuring that the right entity has control over the contract settings and operations.

## Code for updatesetting


```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "owner_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
  "brokerage": 20,
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```
## Common Errors

Common Errors  
When using the updatesetting HTTP REST API Tron method, the following issues may occur:  
- **Invalid Address Format**: The provided `owner_address` is not in a valid Tron address format. Ensure the address starts with a 'T' and is 34 characters long.  
- **Invalid Brokerage Value**: The `brokerage` percentage is outside the acceptable range of 0 to 100. Adjust the value to be within this range to proceed.  
- **Visibility Parameter Missing**: The `visible` parameter is not included in the request. Ensure that you include this parameter to specify the visibility status of the setting.  
- **Unauthorized Access**: The API call may fail if the user does not have permission to update the settings. Verify that the correct authentication credentials are used.

Using the updatesetting method in Web3 applications allows developers to programmatically manage and customize settings on the Tron network, enhancing automation and efficiency. This functionality is crucial for maintaining dynamic and responsive blockchain applications that can adapt to changing requirements and user needs.

### conclusion

The UpdateSetting HTTP API on Tron allows users to modify settings such as brokerage and visibility efficiently. By providing the owner's address and specifying the desired changes, like setting brokerage to 20 and making the visibility true, users can easily manage their configurations via the UpdateSetting interface.
