# getaccountresource


## Meta Description
Access 'getaccountresource' via Tron’s RESTful API Interface for efficient account resource management.

## Description
The 'getaccountresource' Web3 method in the Tron protocol is a powerful tool designed for developers seeking detailed insights into account resource allocations. This RESTful API Interface allows users to retrieve comprehensive data on resources such as bandwidth and energy linked to any account on the Tron network. By utilizing the 'getaccountresource RPC protocol', developers can efficiently manage and optimize resource usage, ensuring seamless interaction with smart contracts and decentralized applications. This method is integral for maintaining optimal performance and resource allocation, making it an essential component for developers working within the Tron ecosystem. With its technical precision and user-friendly design, 'getaccountresource' streamlines resource management in a decentralized environment.

## Supported Networks
The getaccountresource REST API method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters getaccountresource method needs to be executed.

- **address** (required)
  - **Type**: String
  - **Description**: The account address for which resources are being queried.
  - **Default/Supported Values**: A valid account address, typically a string of alphanumeric characters.

- **visible** (optional)
  - **Type**: Boolean
  - **Description**: Determines whether the response should be in a human-readable format.
  - **Default/Supported Values**: `true` or `false`. Default is `false` if not specified.

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using getaccountresource

Request
```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/getaccountresource \
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
  "TotalNetWeight": 26514583140,
  "EnergyLimit": 10,
  "TotalEnergyLimit": 180000000000,
  "TotalEnergyWeight": 16551109184
}
```
## Body Parameters

Here is the list of body parameters for the `getaccountresource` method:

1. **freeNetUsed** (`int64`)  
   Free bandwidth used

2. **freeNetLimit** (`int64`)  
   The amount of free net bandwidth available  
   *Example value: 600*

3. **NetUsed** (`int64`)  
   Used amount of bandwidth obtained by staking

4. **NetLimit** (`int64`)  
   Total bandwidth obtained by staking

5. **TotalNetLimit** (`int64`)  
   Total bandwidth can be obtained by staking by the whole network  
   *Example value: 43,200,000,000*

6. **TotalNetWeight** (`int64`)  
   Total TRX staked for bandwidth by the whole network  
   *Example value: 26,514,583,140*

7. **totalTronPowerWeight** (`int64`)  
   The total amount of voting rights obtained by the whole network

8. **tronPowerLimit** (`int64`)  
   TRON Power (vote)

9. **tronPowerUsed** (`int64`)  
   TRON Power (vote) used

10. **EnergyUsed** (`int64`)  
    Energy used

11. **EnergyLimit** (`int64`)  
    Total energy obtained by staking  
    *Example value: 10*

12. **TotalEnergyLimit** (`int64`)  
    Total energy can be obtained by staking by the whole network  
    *Example value: 180,000,000,000*

13. **TotalEnergyWeight** (`int64`)  
    Total TRX staked for energy by the whole network  
    *Example value: 16,551,109,184*

14. **assetNetUsed** (`map<string, int64>`)  
    The amount of free bandwidth of each TRC10 asset used by the account  
    *Example:*
    ```json
    [
      {"key": "1004977", "value": 0},
      {"key": "1005026", "value": 0}
    ]
    ```

15. **assetNetLimit** (`map<string, int64>`)  
    The amount of free bandwidth for each TRC10 asset in the account  
    *Example:*
    ```json
    [
      {"key": "1004977", "value": 1},
      {"key": "1005026", "value": 0}
    ]
    ```

## Use Case

Here are some use-cases for the `getaccountresource` method in Web3 programming:

1. **Resource Management and Optimization**: In blockchain platforms that utilize resource models, such as TRON or EOS, the `getaccountresource` method can be instrumental in managing and optimizing an account's resource allocation. Developers can use this method to retrieve detailed information about an account's current resource status, such as bandwidth, energy, or CPU usage. This insight allows developers to optimize smart contract interactions by ensuring that sufficient resources are available for transactions, thereby preventing failed transactions due to resource exhaustion.

2. **Monitoring Account Activity**: Another practical application of the `getaccountresource` method is in monitoring the activity and health of blockchain accounts. By regularly querying an account's resource usage, developers and users can detect unusual patterns or spikes in resource consumption, which might indicate malicious activities or inefficiencies in smart contract execution. This proactive monitoring can enhance security and performance by allowing timely interventions to address potential issues.

3. **Cost Estimation and Budgeting**: For developers and businesses operating on blockchain platforms, understanding the cost implications of transactions is crucial. The `getaccountresource` method can provide insights into the resource consumption associated with specific transactions or smart contracts. By analyzing this data, developers can estimate future resource needs and budget accordingly, ensuring that they maintain a balance between operational costs and resource availability. This is particularly useful for applications with high transaction volumes or those that require precise cost management.

## Code for getaccountresource


```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/getaccountresource"
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
When using the getaccountresource HTTP REST API Tron method, the following issues may occur:  
- Invalid Address Format: If the provided address does not adhere to the Tron address format, the request will fail. Ensure the address starts with 'T' and is 34 characters long.  
- Address Not Found: The specified address may not exist on the Tron network. Double-check the address for typos, or ensure the account has been activated on the blockchain.  
- Network Connectivity Issues: Poor internet connection or network misconfigurations can lead to request timeouts. Verify your network settings and ensure a stable connection to the Tron network.  
- Insufficient Permissions: If the API key or credentials lack the necessary permissions, access will be denied. Ensure your API key has the correct scope for accessing account resources.  

Using the getaccountresource method in Web3 applications provides developers with crucial insights into the resource consumption and bandwidth availability of Tron accounts. This information is vital for optimizing transaction efficiency and managing smart contract interactions effectively.

### conclusion

The `getaccountresource` HTTP API in Tron provides comprehensive insights into an account's resources, such as bandwidth and energy. By accessing this API, users can efficiently manage and monitor their account status, ensuring optimal performance on the Tron network.
