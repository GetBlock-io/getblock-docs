---
description: >-
  The getSupply JSON-RPC method retrieves information about the current supply
  of Solana (SOL), including total, circulating, and non-circulating supplies.
---

# getSupply – Solana

{% hint style="success" %}
The getSupply RPC Solana method provides details on the current supply distribution in the Solana blockchain.
{% endhint %}

It supports an optional parameters object for customizing the request, such as excluding non-circulating account details for performance optimization.

This method is useful for developers building dashboards, analytics tools, and financial services that rely on accurate blockchain supply data via Solana's Core API.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Optional Parameters

* object (optional): A configuration object containing:
  * commitment (string, optional): Defines the level of finality for the request.
  * excludeNonCirculatingAccountsList (bool, optional): If set to true, excludes the list of non-circulating accounts from the response.

### Result

The response returns an RpcResponse object containing:

* context (object): Contextual information about the slot.
  * slot (u64): The slot number when the supply data was retrieved.
* value (object):
  * total (u64): Total supply of SOL in lamports.
  * circulating (u64): Circulating supply in lamports.
  * nonCirculating (u64): Non-circulating supply in lamports.
  * nonCirculatingAccounts (array): An array of account addresses holding non-circulating supply.

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
    "method": "getSupply"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the current supply information.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1114
    },
    "value": {
      "circulating": 16000,
      "nonCirculating": 1000000,
      "nonCirculatingAccounts": [
        "FEy8pTbP5fEoqMV1GdTz83byuA8EKByqYat1PKDgVAq5",
        "9huDUZfxoJ7wGMTffUE7vh1xePqef7gyrLJu9NApncqA",
        "3mi1GmwEE3zo2jmfDuzvjSX9ovRXsDUKHvsntpkhuLJ9",
        "BYxEJTDerkaRWBem3XgnVcdhppktBXa2HbkHPKj2Ui4Z"
      ],
      "total": 1016000
    }
  },
  "id": 1
}
```

### Error Handling

Common getSupply error scenarios:

* Invalid parameters: If the request contains malformed or incorrect parameters.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid parameter format"
  },
  "id": 1
}
```

### Use Cases

The Solana getSupply method is useful for:

* Validators and node operators: Monitoring total and circulating supply.
* Wallet applications: Displaying supply metrics to users.
* Web3 analytics tools: Analyzing supply trends and distributions.
* Blockchain explorers: Displaying real-time supply statistics.

### Code Example – Web3 getSupply Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getSupply"
};

const fetchSupplyInfo = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result?.value) {
      const supply = response.data.result.value;
      console.log("Supply Information:");
      console.log(`  Total Supply: ${supply.total}`);
      console.log(`  Circulating Supply: ${supply.circulating}`);
      console.log(`  Non-Circulating Supply: ${supply.nonCirculating}`);
      console.log(`  Non-Circulating Accounts: ${supply.nonCirculatingAccounts.join(', ') || 'N/A'}`);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getSupply error:", error.response?.data || error.message);
  }
};

fetchSupplyInfo();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating getSupply into Solana’s Core API, developers can track blockchain supply metrics, analyze transaction activity, and monitor circulating and non-circulating supply dynamics. This JSON-RPC method is essential for applications that need accurate supply information for financial reporting and analytics.

\
