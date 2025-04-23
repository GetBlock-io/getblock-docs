# getaccount


## Meta Description
Retrieve account details using the 'getaccount' method via Tron’s RESTful API Interface.

## Description
The 'getaccount' Web3 method in the Tron protocol provides a seamless way to retrieve detailed account information using the RESTful API interface. This method is integral for developers needing access to account-specific data, such as balance, bandwidth, and asset holdings. By leveraging the 'getaccount' RPC protocol, users can efficiently query the Tron blockchain for real-time account status and transaction history. The user-friendly design of this API method ensures that both novice and experienced developers can integrate Tron blockchain functionalities into their applications with ease. Whether you're building decentralized applications or conducting blockchain analysis, the 'getaccount' method offers a reliable and efficient solution for accessing comprehensive account data.

## Supported Networks
The getaccount REST API method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters getaccount method needs to be executed.

- **address** (required)
  - **Type**: String
  - **Description**: The address of the account to be queried.
  - **Supported Values**: A valid blockchain address, e.g., "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g".

- **visible** (optional)
  - **Type**: Boolean
  - **Description**: Determines if the account information should be returned in a visible format.
  - **Default Value**: false
  - **Supported Values**: true or false

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using getaccount

Request
```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/getaccount \
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
  "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "balance": 85623170,
  "create_time": 1675582485000,
  "latest_consume_time": 1712481906000,
  "net_window_size": 28800000,
  "net_window_optimized": true,
  "account_resource": {
    "latest_consume_time_for_energy": 1734600927000,
    "energy_window_size": 28800000,
    "acquired_delegated_frozenV2_balance_for_energy": 1000000,
    "energy_window_optimized": true
  },
  "owner_permission": {
    "permission_name": "owner",
    "threshold": 1,
    "keys": [
      {
        "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
        "weight": 1
      }
    ]
  },
  "active_permission": [
    {
      "type": "Active",
      "id": 2,
      "permission_name": "active",
      "threshold": 1,
      "operations": "7fff1fc0033e0300000000000000000000000000000000000000000000000000",
      "keys": [
        {
          "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
          "weight": 1
        }
      ]
    }
  ],
  "frozenV2": [
    {},
    {
      "type": "ENERGY"
    },
    {
      "type": "TRON_POWER"
    }
  ],
  "assetV2": [
    {
      "key": "1004977",
      "value": 8888880000
    },
    {
      "key": "1005026",
      "value": 8888880000
    }
  ],
  "free_asset_net_usageV2": [
    {
      "key": "1004977",
      "value": 0
    },
    {
      "key": "1005026",
      "value": 0
    }
  ],
  "asset_optimized": true
}
```
## Body Parameters

Here is the list of body parameters for the `getaccount` method:

1. **address**: The unique address identifier for the account, e.g., "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g".

2. **balance**: The current balance of the account in the smallest unit of the cryptocurrency, e.g., 85623170.

3. **create_time**: The timestamp indicating when the account was created, e.g., 1675582485000.

4. **latest_consume_time**: The timestamp of the latest consumption activity, e.g., 1712481906000.

5. **net_window_size**: The size of the net window, e.g., 28800000.

6. **net_window_optimized**: A boolean indicating if the net window is optimized, e.g., true.

7. **account_resource**: An object containing details about the account's resources:
   - **latest_consume_time_for_energy**: Timestamp of the latest energy consumption, e.g., 1734600927000.
   - **energy_window_size**: The size of the energy window, e.g., 28800000.
   - **acquired_delegated_frozenV2_balance_for_energy**: The balance acquired for energy through delegation, e.g., 1000000.
   - **energy_window_optimized**: A boolean indicating if the energy window is optimized, e.g., true.

8. **owner_permission**: An object detailing the owner's permission settings:
   - **permission_name**: The name of the permission, e.g., "owner".
   - **threshold**: The threshold value for permission, e.g., 1.
   - **keys**: A list of keys associated with the permission, each containing:
     - **address**: The address linked to the key, e.g., "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g".
     - **weight**: The weight of the key, e.g., 1.

9. **active_permission**: A list containing active permissions:
   - **type**: The type of permission, e.g., "Active".
   - **id**: The identifier for the permission, e.g., 2.
   - **permission_name**: The name of the permission, e.g., "active".
   - **threshold**: The threshold value for the permission, e.g., 1.
   - **operations**: A string representing allowed operations, e.g., "7fff1fc0033e0300000000000000000000000000000000000000000000000000".
   - **keys**: A list of keys associated with the permission, each containing:
     - **address**: The address linked to the key, e.g., "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g".
     - **weight**: The weight of the key, e.g., 1.

10. **frozenV2**: A list detailing frozen assets:
    - **type**: The type of frozen asset, e.g., "ENERGY", "TRON_POWER".

11. **assetV2**: A list of assets associated with the account:
    - **key**: The unique identifier for the asset, e.g., "1004977".
    - **value**: The value of the asset, e.g., 8888880000.

12. **free_asset_net_usageV2**: A list detailing the net usage of free assets:
    - **key**: The unique identifier for the asset, e.g., "1004977".
    - **value**: The net usage value, e.g., 0.

13. **asset_optimized**: A boolean indicating if the assets are optimized, e.g., true.

## Use Case

Here are some use-cases for the `getaccount` method in Web3 programming:

1. **Account Balance Retrieval**: One of the primary uses of the `getaccount` method is to retrieve the balance of a specific account on the blockchain. This can be crucial for applications that need to display user balances, manage funds, or trigger specific actions based on account holdings. For instance, a decentralized finance (DeFi) application might use this method to check if a user has sufficient funds before allowing them to participate in a lending or borrowing protocol.

2. **Transaction Monitoring**: Developers can use the `getaccount` method to monitor account activity, including incoming and outgoing transactions. This is particularly useful for creating alert systems or dashboards that track the flow of assets in real-time. For example, a cryptocurrency exchange might implement this method to keep track of deposits and withdrawals, ensuring that transactions are processed and reflected in user accounts promptly.

3. **User Authentication and Verification**: In Web3 applications, the `getaccount` method can be employed for user authentication by verifying ownership of an account. When a user connects their wallet to a dApp, the application can use this method to confirm the user's identity by checking their account details against known records. This is essential for ensuring secure access and personalized experiences within decentralized applications.

## Code for getaccount


```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/getaccount"
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

Common Errors  
When using the getaccount HTTP REST API Tron method, the following issues may occur:  
- **Invalid Address Format**: If the address provided does not conform to the Tron address format, the request will fail. Ensure the address starts with a 'T' and is 34 characters long.  
- **Network Connectivity Issues**: If there is a problem with the network connection, the request may time out or fail. Verify your internet connection and try again.  
- **Account Not Found**: If the account does not exist on the Tron blockchain, the method will return an error. Double-check the address for typos or incorrect information.  
- **Insufficient Permissions**: If the API key or token used does not have the required permissions, the request will be denied. Ensure your API credentials are correctly configured and have the necessary access rights.  

Using the getaccount method in Web3 applications provides real-time access to account information, enabling developers to build responsive and interactive blockchain applications. It is a crucial tool for verifying account balances, transaction histories, and other essential details, enhancing the functionality and user experience of decentralized applications.

### conclusion

The `getaccount` HTTP API in Tron allows users to retrieve detailed information about a specific account, such as the address "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g" mentioned. This API is essential for developers and users who need to access account data programmatically while ensuring visibility and transparency within the Tron blockchain network.
