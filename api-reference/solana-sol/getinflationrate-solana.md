---
description: >-
  The getInflationRate JSON-RPC method retrieves the specific inflation values
  for the current epoch in the Solana blockchain.
---

# getInflationRate – Solana

{% hint style="success" %}
The getInflationRate RPC Solana method provides real-time inflation data for the current epoch, including the total inflation percentage, the amount allocated to validators, and the portion allocated to the foundation.&#x20;
{% endhint %}

The getInflationRate method returns the current inflation rate parameters of the Solana network. It provides information on the expected annual inflation rate, staking rewards distribution, and total token supply adjustments, helping users and validators understand the network’s monetary policy.

### Supported Networks

This method is accessible through Solana API endpoints:

* Mainnet
* Devnet

### Parameters

{% hint style="info" %}
This method does not require any parameters.
{% endhint %}

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
    "method": "getInflationRate"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful getInflationRate example response returns the inflation rate breakdown for the current epoch.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "epoch": 100,
    "foundation": 0.001,
    "total": 0.149,
    "validator": 0.148
  },
  "id": 1
}
```

### Error Handling

Common getInflationRate error scenarios:

* Network connectivity issues: The request fails due to API unavailability.
* Node synchronization issues: If the node is out of sync, it may return outdated inflation values.
* Invalid response format: An unexpected JSON structure may indicate an internal API error.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Inflation rate data unavailable"
  },
  "id": 1
}
```

### Use Cases

The Solana getInflationRate method is essential for:

* Validators: Monitoring expected staking rewards from inflation.
* Blockchain explorers: Displaying real-time inflation data for each epoch.
* Web3 applications: Adjusting economic models based on inflation distribution.
* Analytics platforms: Tracking inflation trends and network sustainability.

### Code Example – Web3 getInflationRate Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getInflationRate"
};

const fetchInflationRate = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      const rate = response.data.result;
      console.log(`Inflation Rate:`);
      console.log(`Total: ${rate.total}`);
      console.log(`Validator: ${rate.validator}`);
      console.log(`Foundation: ${rate.foundation}`);
      console.log(`Epoch: ${rate.epoch}`);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getInflationRate error:", error.response?.data || error.message);
  }
};

fetchInflationRate();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getInflationRate API with Solana’s Core API to retrieve real-time inflation rate data dynamically. By leveraging JSON-RPC parameters and endpoints, developers can ensure accurate tracking of staking rewards, inflation changes, and the impact of network economics on the Solana ecosystem.

\
