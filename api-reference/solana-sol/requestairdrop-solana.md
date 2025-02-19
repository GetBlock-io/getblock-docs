---
description: >-
  The requestAirdrop JSON-RPC method allows users to request an airdrop of
  lamports to a specified Solana Pubkey.
---

# requestAirdrop – Solana

{% hint style="success" %}
The requestAirdrop RPC Solana method facilitates funding test accounts with lamports.&#x20;
{% endhint %}

This is essential for developers building and testing applications on Solana Devnet without requiring real funds. The airdrop is sent as a transaction to the provided account.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet (limited availability)
* Devnet (primary environment for airdrops)

### Parameters

#### Required Parameters

* string (required): The Pubkey of the account to receive the airdrop. This should be a base-58 encoded string.
* integer (required): The number of lamports to airdrop, provided as a u64 integer.

#### Optional Parameters

* object (optional): A configuration object containing:
  * commitment (string): Defines the level of finality for the request.

### Result

The response returns a transaction signature of the airdrop.

#### Result Format

* string: The transaction signature as a base-58 encoded string.

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
    "method": "requestAirdrop",
    "params": [
      "83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri",
      1000000000
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the transaction signature of the airdrop.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": "5VERv8NMvzbJMEkV8xnrLkEaWRtSz9CosKDYjCJjBRnbJLgp8uirBgmQpjKhoR4tjF3ZpRzrFmBV6UjKdiSZkQUW",
  "id": 1
}
```

In this response:

* result: The transaction signature confirming the airdrop.

### Error Handling

Common requestAirdrop error scenarios:

* Invalid Pubkey: If the provided Pubkey is invalid or not base-58 encoded.
* Network issues: Connectivity problems with the Solana JSON-RPC API endpoints.
* Insufficient faucet funds: When the Solana faucet runs out of lamports.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid Pubkey"
  },
  "id": 1
}
```

### Use Cases

The Solana requestAirdrop method is useful for:

* Development environments: Funding test accounts on Devnet.
* Blockchain education: Demonstrating transactions without using real funds.
* Web3 applications: Simulating transaction activity.

### Code Example – Web3 requestAirdrop Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key";
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "requestAirdrop",
  params: [
    "83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri",
    1000000000
  ]
};

const requestAirdrop = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      console.log("Airdrop Transaction Signature:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("requestAirdrop error:", error.response?.data || error.message);
  }
};

requestAirdrop();
```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating requestAirdrop into Solana’s Core API, developers can fund test accounts, simulate transactions, and build reliable dApps without relying on real funds. This JSON-RPC method is essential for blockchain development on Solana Devnet.
