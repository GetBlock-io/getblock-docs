---
description: >-
  The getMinimumBalanceForRentExemption JSON-RPC method retrieves the minimum
  balance required to make an account rent-exempt in the Solana network.
---

# getMinimumBalanceForRentExemption – Solana

{% hint style="success" %}
The **getMinimumBalanceForRentExemption** RPC Solana method calculates the minimum required balance for a given account size to avoid rent collection.
{% endhint %}

The getMinimumBalanceForRentExemption method returns **the minimum SOL balance required to make an account rent-exempt** based on its data size. This ensures that the account remains active without needing periodic rent payments, making it essential for managing account storage on the Solana blockchain.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* **`usize`** (optional): The length of the account’s data (in bytes).

#### Optional Parameters

* **`object`** (optional): A configuration object containing:
  * **commitment** (`string`, optional): Specifies the level of finality.

### Result

The response returns a `u64` value, representing the **minimum number of lamports required for the account to remain rent-free**.

### Request Example

#### API Endpoints

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### cURL Example

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getMinimumBalanceForRentExemption",
    "params": [50]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the minimum balance required for an account to remain rent-free.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": 500,
  "id": 1
}
```

### Error Handling

Common getMinimumBalanceForRentExemption error scenarios:

* Invalid account size: If an incorrect or negative value is provided.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.
* Malformed request: If the request format is incorrect.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid account size"
  },
  "id": 1
}
```

### Use Cases

The Solana **getMinimumBalanceForRentExemption** method is useful for:

* **dApp developers**: Ensuring that accounts have enough balance to avoid rent charges;
* **Web3 analytics tools**: Estimating storage costs for Solana-based applications;
* **Validators and infrastructure providers**: Monitoring rent-exempt accounts and optimizing network resources.

### Code getMinimumBalanceForRentExemption Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getMinimumBalanceForRentExemption",
  params: [50]
};

const fetchMinimumBalanceForRentExemption = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result !== undefined) {
      console.log("Minimum Rent-Exempt Balance (lamports):", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getMinimumBalanceForRentExemption error:", error.response?.data || error.message);
  }
};

fetchMinimumBalanceForRentExemption();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 **getMinimumBalanceForRentExemption** into Solana’s Core API, developers can manage account rent requirements efficiently, optimize block storage, and ensure smooth transaction execution. The JSON-RPC request allows easy retrieval of rent-exemption values, making it a key component in decentralized applications, wallets, and staking infrastructure.

\
