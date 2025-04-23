# undelegateresource


## Meta Description
Explore the 'undelegateresource' method in Tron's RESTful API Interface for efficient resource management.

## Description
The 'undelegateresource' Web3 method in the Tron protocol allows users to efficiently manage their delegated resources. This RESTful API Interface provides a seamless way to undelegate resources previously allocated to other accounts, offering users greater control over their TRX and bandwidth. When using the 'undelegateresource' RPC protocol, developers can interact programmatically with the Tron blockchain, enabling the automation of resource management tasks. This method is essential for developers aiming to optimize their decentralized applications by ensuring that resources are allocated where they are most needed. By integrating this API into your workflow, you can enhance your application's responsiveness and resource efficiency, ultimately improving the user experience.

## Supported Networks
The undelegateresource REST API method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters undelegateresource method needs to be executed.

- **owner_address**: 
  - **Type**: String
  - **Description**: The address of the owner who is undelegating the resource.
  - **Required**: Yes
  - **Default/Supported Values**: Must be a valid blockchain address.

- **receiver_address**: 
  - **Type**: String
  - **Description**: The address of the receiver from whom the resource is being undelegated.
  - **Required**: Yes
  - **Default/Supported Values**: Must be a valid blockchain address.

- **balance**: 
  - **Type**: Integer
  - **Description**: The amount of resource to be undelegated.
  - **Required**: Yes
  - **Default/Supported Values**: Must be a positive integer.

- **resource**: 
  - **Type**: String
  - **Description**: The type of resource being undelegated.
  - **Required**: Yes
  - **Default/Supported Values**: Supported values include "BANDWIDTH".

- **visible**: 
  - **Type**: Boolean
  - **Description**: Indicates whether the transaction should be visible.
  - **Required**: Optional
  - **Default/Supported Values**: Defaults to `true` if not specified.

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using undelegateresource

Request
```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/ \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "receiver_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
  "balance": 1000000,
  "resource": "BANDWIDTH",
  "visible": true
}
```

Response
```json

{
  "Error": "class org.tron.core.exception.ContractValidateException : delegated Resource does not exist"
}
```
## Body Parameters

Here is the list of body parameters for the `undelegateresource` method:

1. **owner_address**: The address of the account that originally delegated the resource.
2. **receiver_address**: The address of the account that was delegated the resource.
3. **resource**: The type of resource being undelegated, such as bandwidth or energy.
4. **amount**: The amount of resource to undelegate.
5. **permission_id**: (Optional) The ID of the permission used for this transaction, if applicable.
6. **response**: Contains the status of the undelegation operation, including any error messages or confirmations.

## Use Case

Here are some use-cases for the undelegateresource method in Web3 programming:

1. **Resource Reallocation**: In a decentralized application (dApp), developers might need to reallocate resources such as bandwidth or energy to optimize performance or manage costs effectively. By using the undelegateresource method, a user can withdraw previously delegated resources from one address and potentially delegate them to another address that requires more resources. This flexibility allows for dynamic resource management, ensuring that the dApp runs efficiently and cost-effectively.

2. **Security and Control**: Users may choose to undelegate resources to regain control over their assets for security reasons. If a user suspects that their delegated resources are being misused or if there is a potential security threat to the receiver address, they can use the undelegateresource method to withdraw their resources promptly. This action helps in maintaining control over one's assets and mitigating risks associated with unauthorized or malicious use.

3. **Resource Expiration Management**: In some blockchain networks, delegated resources might have an expiration period after which they need to be redelegated or undelegated. The undelegateresource method can be employed to manage these expiration periods effectively. By undelegating resources before they expire, users can avoid potential penalties or loss of resources, ensuring that they maintain optimal resource allocation and usage over time.

## Code for undelegateresource


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
  "receiver_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
  "balance": 1000000,
  "resource": "BANDWIDTH",
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```
## Common Errors

Common Errors  
When using the undelegateresource HTTP REST API Tron method, the following issues may occur:  
- **Invalid Address Format**: If the owner or receiver address is not in the correct format, the transaction will fail. Ensure that both addresses conform to the Tron address format, typically starting with a 'T' and followed by alphanumeric characters.  
- **Insufficient Balance**: The undelegation request may be rejected if the balance specified is higher than what is available. Double-check the balance and ensure it does not exceed the delegated amount.  
- **Resource Type Mismatch**: Specifying an incorrect resource type, such as "BANDWIDTH" instead of "ENERGY", will result in an error. Verify that the resource type matches the one currently delegated.  
- **Visibility Parameter Misconfiguration**: If the 'visible' parameter is incorrectly set, it may lead to unexpected behavior in the transaction visibility. Ensure the 'visible' field is correctly set to either true or false as required for your application.

Using the undelegateresource method in Web3 applications allows developers to efficiently manage resource allocations within the Tron network. This functionality enables dynamic resource management, optimizing resource utilization and enhancing application performance by allowing users to reclaim and reallocate resources as needed.

### conclusion

The provided JSON data represents a request to undelegate resources, specifically BANDWIDTH, from one account to another on the Tron blockchain. Utilizing the undelegateresource HTTP API on Tron, users can manage their resource allocations efficiently, ensuring optimal use of their account's capabilities. This action highlights the flexibility and control offered by Tron’s blockchain infrastructure.
