# getassetissuebyname


## Meta Description
Query asset details by name with 'getassetissuebyname' in the Tron JSON-RPC API Interface. Fast, reliable, and developer-friendly.

## Description
The 'getassetissuebyname' method in the Tron protocol allows developers to retrieve detailed information about a specific asset (TRC10 token) by its name using the JSON-RPC API. This Web3-compatible endpoint is part of the Tron RPC protocol, enabling seamless integration with decentralized applications (dApps) and blockchain tools. By supplying the asset name, users can fetch metadata such as issuer address, total supply, precision, and more. Ideal for wallets, explorers, and smart contracts, 'getassetissuebyname RPC protocol' ensures efficient asset lookup without requiring direct blockchain queries. Supports both mainnet and testnet environments.

## Supported Networks
The getassetissuebyname RPC method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters the `getassetissuebyname` method needs to be executed:

- **address** (Required, *string*):  
  The account address for which the asset information is requested.  
  Example: `"TCuM8e98jmPwT1RU2jW7dekUC5HpXbGzFG"`

- **visible** (Optional, *boolean*):  
  Determines whether the address is in base58 format (visible) or hex format (not visible).  
  Default: `false`  
  Supported values: `true` or `false`  

The request is in JSON-RPC format, and both parameters are included in the request body.

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using getassetissuebyname

Request
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' 
--header 'Content-Type: application/json' 
{
  "address": "TCuM8e98jmPwT1RU2jW7dekUC5HpXbGzFG",
  "visible": true
}
```

Response
```json

  "Error": "class java.lang.NullPointerException : null"
}
```
## Body Parameters

Based on the error message provided (`"Error": "class java.lang.NullPointerException : null"}`), it appears to be a runtime exception indicating a null reference was encountered. However, the error does not directly provide information about the method's parameters.

Since you've asked for a structured response about method parameters, here's the general format you requested for the `getassetissuebyname` method:

1. If the method has parameters:  
   **"Here is the list of body parameters for getassetissuebyname method:"**  
   (followed by a numbered list if parameters exist).

2. If no parameters are required:  
   **"This method does not require any parameters."**

---  
**Note:** Without additional context (e.g., API documentation, code snippet, or parameter details), I cannot definitively list the parameters for `getassetissuebyname`. If you provide more details, I can refine the answer.  

The `NullPointerException` suggests the issue might involve an uninitialized object or missing data when calling the method, but this is unrelated to the parameter list.

## Use Case

Here are some use-cases for the `getassetissuebyname` method in Web3 programming:

1. **Token Information Retrieval**  
   This method is commonly used to fetch detailed information about a specific token (TRC10/TRC20) on the TRON blockchain by its name. Developers can retrieve metadata such as token supply, issuer address, precision, and creation timestamp, which is essential for wallets, explorers, or dApps displaying token details.

2. **Token Verification**  
   Before accepting or interacting with a token, applications can use this method to verify its legitimacy. By checking the issuer address and other attributes, smart contracts or DeFi platforms can prevent scams or unauthorized tokens from being used in transactions.

3. **Dynamic Asset Handling**  
   dApps can dynamically adjust their behavior based on the properties of a token. For example, a decentralized exchange (DEX) might use this method to validate token names before listing them or to apply specific trading rules (e.g., fees) depending on the token's attributes.  

This method simplifies asset management and enhances security in TRON-based Web3 applications.

## Code for getassetissuebyname


```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
  "address": "TCuM8e98jmPwT1RU2jW7dekUC5HpXbGzFG",
  "visible": true
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
When using the `getassetissuebyname` RPC Tron method, the following issues may occur:  
- **Invalid or misspelled asset name**: The method requires the exact asset name as issued on-chain. Verify the name matches the token's `name` field exactly, including capitalization and symbols.  
- **Asset not found**: The specified asset may not exist or may have been unlisted. Check the asset's issuance status using a block explorer like Tronscan.  
- **Incorrect address visibility**: If `visible` is set to `false` for a private asset, ensure the requesting account has permission to view it or toggle `visible` to `true` for public assets.  
- **RPC endpoint issues**: Connectivity or misconfigured nodes can cause timeouts. Use a reliable Tron RPC provider or retry with a different endpoint.  

The `getassetissuebyname` method is essential for Web3 apps to query token metadata (e.g., supply, precision) directly from the blockchain, enabling dynamic asset displays or compliance checks. Its efficiency simplifies integration for wallets, DEXs, and analytics tools.

### conclusion

The provided JSON is a request to query an address on the Tron blockchain with the `visible` parameter set to `true`.  

To fetch details about a specific TRC10 or TRC20 token, you can use the **getassetissuebyname** RPC method in Tron. This method allows developers to retrieve asset information by its name, making it useful for tracking token metadata, supply, and issuer details. By integrating **getassetissuebyname** into your Tron applications, you can efficiently manage token-related queries and enhance blockchain interactions. The Tron RPC methods, including **getassetissuebyname**, provide powerful tools for accessing on-chain data programmatically.
