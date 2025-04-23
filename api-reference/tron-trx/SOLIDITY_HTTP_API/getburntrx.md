---
description: >-
  Query burnt TRX details with 'getburntrx' via the Tron HTTP REST API
  Interface. Technical, efficient, and developer-focused.
---

# getburntrx - TRON

## Description

The 'getburntrx' HTTP REST API method in the Tron protocol allows developers to retrieve detailed information about burnt TRX tokens, including transaction hashes and amounts. This Web3-compatible endpoint is part of the Tron HTTP REST API, enabling seamless integration with decentralized applications (dApps) and blockchain analytics tools. The 'getburntrx' HTTP REST API protocol supports querying historical and real-time data, making it essential for tracking tokenomics, auditing, and compliance. Ideal for developers building on Tron, this method ensures accurate, on-chain verification of TRX burn events. Documentation includes request/response examples for easy implementation.

## Supported Networks

The getburntrx HTTP REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

{% hint style="info" %}
This method does not require any parameters
{% endhint %}

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using getburntrx

Request

```json
curl --request GET \
     --url https://api.shasta.trongrid.io/walletsolidity/getburntrx \
     --header 'accept: application/json'
```

Response

```json
{
  "burnTrxAmount": 109556997154520
}
```

## Body Parameters

burnTrxAmount - int64 : Amount of TRX burned, in sun.

## Use Case

Here are some use-cases for the `getburntrx` method in Web3 programming:

1. **Transaction Fee Analysis**\
   Developers can use `getburntrx` to retrieve details about the TRX burned (destroyed) as transaction fees on the TRON blockchain. This helps in analyzing network congestion, estimating costs for dApps, or optimizing gas fees for users.
2. **Supply Tracking & Tokenomics**\
   Since TRX is burned to process transactions, this method allows projects to monitor the deflationary impact on TRX's total supply. It’s useful for tokenomics models, staking mechanisms, or auditing burn-related contract logic (e.g., in DeFi protocols).
3. **Smart Contract Optimization**\
   By querying burned TRX amounts, developers can benchmark and optimize smart contract efficiency—reducing unnecessary operations that incur higher burn fees, thus improving user experience and cost-effectiveness.

_(Note: Replace `getburntrx` with the actual method name if it differs in your context.)_

## Code for getburntrx

```python
import requests

url = "https://api.shasta.trongrid.io/walletsolidity/getburntrx"
headers = {"accept": "application/json"}

response = requests.get(url, headers=headers)
print(response.json())
```

## Common Errors

**Common Errors**\
When using the `getburntrx` RPC Tron method, the following issues may occur:

* **Invalid pagination parameters**: If `offset` or `limit` values are negative or exceed allowed ranges, the request will fail. Ensure values are non-negative and within reasonable bounds (e.g., `limit` ≤ 100).
* **Network congestion delays**: High Tron network activity may slow response times. Retry the request or adjust timeout settings in your application.
* **Missing or malformed request data**: Omitting required fields like `offset` or `limit` will return an error. Always validate the JSON structure before sending.

The `getburntrx` method is essential for Web3 apps tracking TRX burn events, enabling transparent audit trails and real-time analytics. By integrating this RPC, developers can monitor token economics and enhance user trust in decentralized systems.

### conclusion

Here’s a concise conclusion integrating your keywords:

_GetBurntRX_ is a valuable tool for tracking burnt TRX transactions on the Tron blockchain, offering transparency and efficiency. By leveraging _GetBurntRX RPC_ endpoints, users can seamlessly query data like offsets and limits to monitor token burns. For developers and analysts, _GetBurntRX_ simplifies interactions with the Tron network, ensuring accurate and real-time insights into TRX burn mechanics.

Let me know if you'd like any refinements!
