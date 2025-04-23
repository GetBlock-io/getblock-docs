# getburntrx


## Meta Description
Query burnt TRX details with 'getburntrx' via the Tron JSON-RPC API Interface. Technical, efficient, and developer-focused.

## Description
The 'getburntrx' RPC method in the Tron protocol allows developers to retrieve detailed information about burnt TRX tokens, including transaction hashes and amounts. This Web3-compatible endpoint is part of the Tron JSON-RPC API, enabling seamless integration with decentralized applications (dApps) and blockchain analytics tools. The 'getburntrx' RPC protocol supports querying historical and real-time data, making it essential for tracking tokenomics, auditing, and compliance. Ideal for developers building on Tron, this method ensures accurate, on-chain verification of TRX burn events. Documentation includes request/response examples for easy implementation.

## Supported Networks
The getburntrx RPC method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters the method needs to be executed:

- **offset** (optional, *integer*):  
  Specifies the starting index for pagination. Defaults to `0` if not provided.  
  Example: `0` (first item)  

- **limit** (optional, *integer*):  
  Defines the maximum number of items to return per request. Defaults to `20` if not specified.  
  Example: `20` (returns 20 items)  

**Note**: Both parameters are optional and control pagination in a REST-style request (likely a GET with query parameters). JSON-RPC would typically embed these in a `params` object.

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using getburntrx

Request
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' 
--header 'Content-Type: application/json' 
{
  "offset": 0,
  "limit": 20
}
```

Response
```json

  "burnTrxAmount": 14329329166263960
}
```
## Body Parameters

This method does not require any parameters.

## Use Case

Here are some use-cases for the `getburntrx` method in Web3 programming:

1. **Transaction Fee Analysis**  
   Developers can use `getburntrx` to retrieve details about the TRX burned (destroyed) as transaction fees on the TRON blockchain. This helps in analyzing network congestion, estimating costs for dApps, or optimizing gas fees for users.

2. **Supply Tracking & Tokenomics**  
   Since TRX is burned to process transactions, this method allows projects to monitor the deflationary impact on TRX's total supply. It’s useful for tokenomics models, staking mechanisms, or auditing burn-related contract logic (e.g., in DeFi protocols).

3. **Smart Contract Optimization**  
   By querying burned TRX amounts, developers can benchmark and optimize smart contract efficiency—reducing unnecessary operations that incur higher burn fees, thus improving user experience and cost-effectiveness.  

*(Note: Replace `getburntrx` with the actual method name if it differs in your context.)*

## Code for getburntrx


```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
  "offset": 0,
  "limit": 20
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)
```
## Common Errors

**Common Errors**  
When using the `getburntrx` RPC Tron method, the following issues may occur:  
- **Invalid pagination parameters**: If `offset` or `limit` values are negative or exceed allowed ranges, the request will fail. Ensure values are non-negative and within reasonable bounds (e.g., `limit` ≤ 100).  
- **Network congestion delays**: High Tron network activity may slow response times. Retry the request or adjust timeout settings in your application.  
- **Missing or malformed request data**: Omitting required fields like `offset` or `limit` will return an error. Always validate the JSON structure before sending.  

The `getburntrx` method is essential for Web3 apps tracking TRX burn events, enabling transparent audit trails and real-time analytics. By integrating this RPC, developers can monitor token economics and enhance user trust in decentralized systems.

### conclusion

Here’s a concise conclusion integrating your keywords:  

*GetBurntRX* is a valuable tool for tracking burnt TRX transactions on the Tron blockchain, offering transparency and efficiency. By leveraging *GetBurntRX RPC* endpoints, users can seamlessly query data like offsets and limits to monitor token burns. For developers and analysts, *GetBurntRX* simplifies interactions with the Tron network, ensuring accurate and real-time insights into TRX burn mechanics.  

Let me know if you'd like any refinements!
