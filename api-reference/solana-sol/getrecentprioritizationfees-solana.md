---
description: >-
  The getRecentPrioritizationFees JSON-RPC method retrieves a list of
  prioritization fees from recent blocks.
---

# getRecentPrioritizationFees – Solana

{% hint style="success" %}
The getRecentPrioritizationFees RPC Solana method provides insight into the dynamic fee market within the Solana blockchain.&#x20;
{% endhint %}

It helps developers assess the cost required for prioritizing transactions in recent blocks, ensuring timely transaction execution in high-traffic conditions.

By providing an optional parameters array of account addresses, the response can reflect the estimated fee for a transaction that locks all specified accounts as writable. This feature enhances the flexibility of Solana’s Core API for advanced fee calculations.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Optional Parameters

* array (optional): An array of up to 128 account addresses, provided as base-58 encoded strings.
  * If provided, the response reflects the fee required for a transaction locking all specified accounts as writable.

### Result

The response returns an array of RpcPrioritizationFee objects, each containing:

* slot (u64): The slot in which the prioritization fee was observed.
* prioritizationFee (u64): The per-compute-unit fee paid by at least one successfully landed transaction, measured in micro-lamports (0.000001 lamports).

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
--header "Content-Type: "application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getRecentPrioritizationFees",
    "params": [
      ["CxELquR1gPP8wHe33gZ4QxqGB3sZ9RSwsJ2KshVewkFY"]
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns an array of recent prioritization fees for the specified accounts (or general network fees if no accounts are provided).

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "slot": 348125,
      "prioritizationFee": 0
    },
    {
      "slot": 348126,
      "prioritizationFee": 1000
    },
    {
      "slot": 348127,
      "prioritizationFee": 500
    },
    {
      "slot": 348128,
      "prioritizationFee": 0
    },
    {
      "slot": 348129,
      "prioritizationFee": 1234
    }
  ],
  "id": 1
}
```

### Error Handling

Common getRecentPrioritizationFees error scenarios:

* Invalid account address: If an incorrectly formatted or non-existent account is provided.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.
* Malformed request: Incorrectly structured JSON requests.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid account address format"
  },
  "id": 1
}
```

### Use Cases

The Solana getRecentPrioritizationFees method is useful for:

* dApp developers: Estimating transaction prioritization fees for faster execution.
* Web3 analytics tools: Tracking fee trends over recent blocks.
* Blockchain explorers: Displaying historical prioritization fee data.
* Validators and node operators: Monitoring network fee dynamics and congestion levels.

### Code getRecentPrioritizationFees Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key";
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getRecentPrioritizationFees",
  params: [
    ["CxELquR1gPP8wHe33gZ4QxqGB3sZ9RSwsJ2KshVewkFY"]
  ]
};

const fetchRecentPrioritizationFees = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && Array.isArray(response.data.result)) {
      const fees = response.data.result;
      if (fees.length > 0) {
        fees.forEach((fee, index) => {
          console.log(`Fee ${index + 1}:`);
          console.log(`  Slot: ${fee.slot}`);
          console.log(`  Prioritization Fee: ${fee.prioritizationFee}`);
        });
      } else {
        console.log("No prioritization fees found.");
      }
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getRecentPrioritizationFees error:", error.response?.data || error.message);
  }
};

fetchRecentPrioritizationFees();
java
```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getRecentPrioritizationFees into Solana’s Core API, developers can optimize transaction execution strategies by analyzing recent prioritization transaction fees. The JSON-RPC request provides valuable insights into the dynamic fee structure, helping dApps, validators, and infrastructure services efficiently navigate Solana’s network conditions.

\
