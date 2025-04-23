---
description: >-
  Query asset details by name with 'getassetissuebyname' in the Tron HTTP REST
  API Interface. Fast, reliable, and developer-friendly.
---

# getassetissuebyname - TRON

## Description

The getassetissuebyname method in the Tron protocol allows developers to retrieve detailed information about a specific asset (TRC10 token) by its name using the HTTP REST API. This Web3-compatible endpoint is part of the Tron RPC protocol, enabling seamless integration with decentralized applications (dApps) and blockchain tools. By supplying the asset name, users can fetch metadata such as issuer address, total supply, precision, and more. Ideal for wallets, explorers, and smart contracts, 'getassetissuebyname RPC protocol' ensures efficient asset lookup without requiring direct blockchain queries. Supports both mainnet and testnet environments.

## Supported Networks

The getassetissuebyname HTTP REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

| Parameter | Type     | Description                                                                                                                                 |
| --------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **value** | `string` | <p>The name of the TRC-10 token, represented as a <strong>hex string</strong>.<br><strong>Default</strong>: <code>62747474657374</code></p> |

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using getassetissuebyname

Request

```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getassetissuebyname \
     --header 'content-type: application/json' \
     --data '
{
  "value": "62747474657374"
}
'
```

Response

```json
{
  "owner_address": "412e1934759faf93497d6742208e2e521f7043e2ff",
  "name": "62747474657374",
  "abbr": "62747474657374",
  "total_supply": 1000000000000000,
  "trx_num": 1000000,
  "precision": 6,
  "num": 1000000,
  "start_time": 1575648000000,
  "end_time": 1575734400000,
  "description": "62747474657374",
  "url": "68747470733a2f2f62746673736f7465722e726561646d652e696f2f646f63732f686f772d746f2d6765742d737461727465642d776974682d736f746572",
  "id": "1000001"
}
```

## Body Parameters

* **id** (`string`):\
  The unique identifier (ID) of the token.
* **owner\_address** (`string`):\
  The address of the token issuer.
* **name** (`string`):\
  The full name of the token.
* **abbr** (`string`):\
  The abbreviated name (symbol) of the token.
* **total\_supply** (`int64`):\
  The total number of tokens issued.
* **frozen\_supply** (`FrozenSupply[]`):\
  A list of frozen supplies. Specifies the number of tokens to be frozen, as determined by the issuer at the time of token creation.
* **trx\_num** (`int32`):\
  Part of the token pricing formula. Defines the price along with `num`, where the price is calculated as `trx_num / num`. The unit of `trx_num` is SUN.
* **num** (`int32`):\
  Part of the token pricing formula. Works together with `trx_num` to define the token price. The unit of `trx_num` is SUN.
* **precision** (`int32`):\
  The number of decimal places the token supports.
* **start\_time** (`int64`):\
  The timestamp indicating the start time of the ICO (Initial Coin Offering).
* **end\_time** (`int64`):\
  The timestamp indicating the end time of the ICO.
* **description** (`string`):\
  A description of the token.
* **url** (`string`):\
  The official website URL of the token, represented as a hex string by default.
* **free\_asset\_net\_limit** (`int64`):\
  The maximum limit for free asset bandwidth available per account.
* **public\_free\_asset\_net\_limit** (`int64`):\
  The total public free asset bandwidth limit available for each account.
* **public\_free\_asset\_net\_usage** (`int64`):\
  The total amount of free bandwidth used by all token holders.
* **public\_latest\_free\_net\_time** (`int64`):\
  The timestamp of the latest usage of the token's free bandwidth.

## Use Case

Here are some use-cases for the `getassetissuebyname` method in Web3 programming:

1. **Token Information Retrieval**\
   This method is commonly used to fetch detailed information about a specific token (TRC10/TRC20) on the TRON blockchain by its name. Developers can retrieve metadata such as token supply, issuer address, precision, and creation timestamp, which is essential for wallets, explorers, or dApps displaying token details.
2. **Token Verification**\
   Before accepting or interacting with a token, applications can use this method to verify its legitimacy. By checking the issuer address and other attributes, smart contracts or DeFi platforms can prevent scams or unauthorized tokens from being used in transactions.
3. **Dynamic Asset Handling**\
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
   "value": "62747474657374"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)
```

## Common Errors

**Common Errors**\
When using the `getassetissuebyname` RPC Tron method, the following issues may occur:

* **Invalid or misspelled asset name**: The method requires the exact asset name as issued on-chain. Verify the name matches the token's `name` field exactly, including capitalization and symbols.
* **Asset not found**: The specified asset may not exist or may have been unlisted. Check the asset's issuance status using a block explorer like Tronscan.
* **Incorrect address visibility**: If `visible` is set to `false` for a private asset, ensure the requesting account has permission to view it or toggle `visible` to `true` for public assets.
* **RPC endpoint issues**: Connectivity or misconfigured nodes can cause timeouts. Use a reliable Tron RPC provider or retry with a different endpoint.

The `getassetissuebyname` method is essential for Web3 apps to query token metadata (e.g., supply, precision) directly from the blockchain, enabling dynamic asset displays or compliance checks. Its efficiency simplifies integration for wallets, DEXs, and analytics tools.

### conclusion

The provided JSON is a request to query an address on the Tron blockchain with the `visible` parameter set to `true`.

To fetch details about a specific TRC10 or TRC20 token, you can use the **getassetissuebyname** RPC method in Tron. This method allows developers to retrieve asset information by its name, making it useful for tracking token metadata, supply, and issuer details. By integrating **getassetissuebyname** into your Tron applications, you can efficiently manage token-related queries and enhance blockchain interactions. The Tron RPC methods, including **getassetissuebyname**, provide powerful tools for accessing on-chain data programmatically.
