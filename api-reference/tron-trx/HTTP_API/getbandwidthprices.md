---
description: >-
  Access getbandwidthprices via Tron’s RESTful API Interface for real-time
  bandwidth pricing data.
---

# getbandwidthprices - TRON

## Description

The 'getbandwidthprices' Web3 method in the Tron protocol provides users with real-time data on bandwidth pricing, crucial for developers and network participants managing resources efficiently. By utilizing the 'getbandwidthprices' RPC protocol, users can seamlessly integrate this data into their applications, ensuring they stay informed about current network costs. This method is part of Tron’s comprehensive RESTful API Interface, designed to facilitate easy access to blockchain data and enhance the development experience. Whether you're building dApps or monitoring network activity, 'getbandwidthprices' offers a reliable and straightforward way to obtain the necessary bandwidth pricing information, enabling informed decision-making and optimized resource allocation on the Tron network.

## Supported Networks

The getbandwidthprices REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters getbandwidthprices method needs to be executed.

* **offset**
  * **Type**: Integer
  * **Description**: Specifies the starting point for retrieving data, often used for pagination.
  * **Optional/Required**: Optional
  * **Default/Supported Values**: Default is usually 0. Supported values are non-negative integers.
* **limit**
  * **Type**: Integer
  * **Description**: Determines the maximum number of items to retrieve, often used for pagination.
  * **Optional/Required**: Optional
  * **Default/Supported Values**: Default can vary, commonly 20. Supported values are positive integers.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using getbandwidthprices

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/ \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "offset": 0,
  "limit": 20
}
```

Response

```json

{
  "prices": "0:10,1606240800000:40,1613044800000:140,1627279200000:1000"
}
```

## Body Parameters

Here is the list of body parameters for the getbandwidthprices method:

1. **prices**: A string that represents a series of timestamp and price pairs. Each pair is separated by a comma, with the timestamp and price separated by a colon. The timestamp is in milliseconds since the Unix epoch, and the price is a numerical value representing the cost or price at that specific time.

## Use Case

Here are some use-cases for the `getbandwidthprices` method:

1. **Cost Estimation for DApp Deployment**: In Web3 programming, deploying decentralized applications (DApps) often involves interacting with blockchain networks, which can incur bandwidth costs. The `getbandwidthprices` method can be utilized by developers to estimate the bandwidth costs associated with executing smart contracts or transactions on a particular blockchain. By understanding these costs upfront, developers can better plan and allocate resources, ensuring that their DApp remains cost-effective and efficient.
2. **Network Optimization and Scalability**: For blockchain networks that charge for bandwidth usage, it's crucial to optimize the use of network resources. By using the `getbandwidthprices` method, developers and network operators can monitor current bandwidth prices and adjust their strategies accordingly. This can include optimizing transaction batching, choosing optimal times for executing transactions when prices are lower, or implementing off-chain solutions to reduce on-chain bandwidth usage. Such strategies can lead to improved scalability and reduced operational costs.
3. **User Fee Management**: In scenarios where users of a DApp are responsible for covering transaction fees, the `getbandwidthprices` method can be integrated into the application's user interface to provide real-time fee estimates. This transparency allows users to make informed decisions about when to initiate transactions based on current bandwidth prices, potentially saving them money. Additionally, developers can use this information to implement features like fee alerts or automatic transaction scheduling to help users optimize their spending on network fees.

## Code for getbandwidthprices

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "offset": 0,
  "limit": 20
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

Common Errors\
When using the getbandwidthprices HTTP REST API Tron method, the following issues may occur:

* **Invalid Parameters**: If the `offset` or `limit` parameters are not integers or are set to negative values, the request will fail. Ensure both parameters are positive integers to successfully retrieve bandwidth prices.
* **Rate Limiting**: Excessive requests in a short period may trigger rate limiting, resulting in temporary access denial. Implement exponential backoff or request throttling to manage your API calls efficiently.
* **Network Connectivity Issues**: Poor network connectivity can lead to timeouts or incomplete responses. Verify your network connection and consider implementing retry logic to handle transient network issues.
* **Unauthorized Access**: If API keys or authentication tokens are required and not provided or are incorrect, access to the method will be denied. Ensure your credentials are correct and included in the request headers.

The getbandwidthprices method is instrumental in Web3 applications for accurately assessing the cost of bandwidth on the Tron network, enabling developers to optimize resource allocation and manage transaction fees effectively. By integrating this method, applications can provide users with real-time data on bandwidth pricing, enhancing user experience and ensuring efficient resource management.

### conclusion

The getbandwidthprices HTTP API on the Tron network provides users with valuable insights into bandwidth costs, facilitating better resource management. By utilizing this API, developers can efficiently retrieve data on current bandwidth prices, enabling more informed decision-making and optimized usage within the Tron ecosystem.
