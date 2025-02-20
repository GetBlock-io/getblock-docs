---
description: >-
  The getTokenSupply JSON-RPC method retrieves the total supply of a specific
  SPL Token type.
---

# getTokenSupply – Solana

{% hint style="info" %}
The getTokenSupply RPC Solana method returns the total supply of an SPL Token for a given token Mint address.&#x20;
{% endhint %}

The response includes both the raw token supply and the formatted supply using mint-prescribed decimals. This method helps developers monitor token metrics via Solana’s Core API.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* string (required): The Pubkey of the token Mint to query, provided as a base-58 encoded string.

#### Optional Parameters

* object (optional): A configuration object containing:
  * commitment (string): Defines the level of finality for the request.

### Result

The response returns an RpcResponse object containing:

* context (object): Provides contextual information about the slot.
  * slot (u64): The slot number when the token supply was retrieved.
* value (object):
  * amount (string): The raw total supply (without decimals) as a string.
  * decimals (u8): The number of decimal places for the token.
  * uiAmount (number|null): The formatted token supply with decimals applied (Deprecated).
  * uiAmountString (string): The formatted token supply as a string.

### Request Example

#### API Endpoints

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### cURL Example

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/api-key" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTokenSupply",
    "params": [
      "3wyAj7Rt1TWVPZVteFJPLa26JmLvdb1CAKEFZm3NY75E"
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the total token supply.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1114
    },
    "value": {
      "amount": "100000",
      "decimals": 2,
      "uiAmount": 1000,
      "uiAmountString": "1000"
    }
  },
  "id": 1
}
```

In this response:

* amount: The raw supply as a string.
* decimals: The number of decimal places.
* uiAmountString: The formatted supply as a string.

### Error Handling

Common getTokenSupply error scenarios:

* Invalid Mint Pubkey: If the provided Pubkey is invalid.
* Network issues: Connectivity problems with the Solana JSON-RPC API endpoints.
* Invalid request parameters: Incorrect parameter structure or data types.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid token Mint Pubkey"
  },
  "id": 1
}
```

### Use Cases

The Solana getTokenSupply method is useful for:

* Token analytics: Tracking token supply changes.
* DeFi applications: Monitoring liquidity pool supply.
* Web3 analytics tools: Displaying token metrics to users.
* Wallet applications: Providing token supply information for users.

### Code getTokenSupply Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getTokenSupply",
  params: [
    "3wyAj7Rt1TWVPZVteFJPLa26JmLvdb1CAKEFZm3NY75E"
  ]
};

const fetchTokenSupply = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result?.value) {
      const supply = response.data.result.value;
      console.log("Token Supply Information:");
      console.log(`  Total Supply: ${supply.amount}`);
      console.log(`  Decimals: ${supply.decimals}`);
      console.log(`  UI Amount: ${supply.uiAmount}`);
      console.log(`  UI Amount String: ${supply.uiAmountString}`);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getTokenSupply error:", error.response?.data || error.message);
  }
};

fetchTokenSupply();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getTokenSupply into Solana’s Core API, developers can efficiently track token supply, analyze token metrics, and provide real-time token data for Web3 applications. This JSON-RPC method is crucial for applications that require accurate supply information for tokens on the Solana network.")}

\
