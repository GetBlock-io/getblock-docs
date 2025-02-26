---
description: >-
  The getInflationGovernor JSON-RPC method retrieves the current inflation
  governor settings of the Solana blockchain.
---

# getInflationGovernor – Solana

{% hint style="success" %}
The getInflationGovernor RPC Solana method returns details about the blockchain’s inflation governance, including initial and terminal inflation rates, tapering rates, and the percentage allocated to the foundation.&#x20;
{% endhint %}

This data is critical for understanding Solana’s economic model and how inflation decreases over time.

The request supports an optional parameter:

* commitment (string, optional): Defines the finality level of the retrieved inflation governor parameters.

### Supported Networks

This method is accessible through Solana API endpoints:

* Mainnet
* Devnet

### Parameters

#### Optional Parameters

* commitment (string): Specifies the finality level of the response.

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
    "method": "getInflationGovernor"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful getInflationGovernor example response returns the inflation governance parameters.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "foundation": 0.05,
    "foundationTerm": 7,
    "initial": 0.15,
    "taper": 0.15,
    "terminal": 0.015
  },
  "id": 1
}
```

### Error Handling

Common getInflationGovernor error scenarios:

* Network connectivity issues: The request fails due to API unavailability.
* Invalid request format: If the request parameters are incorrectly structured.
* Node synchronization issues: If the node is not fully synced, it may return outdated inflation values.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Inflation data unavailable"
  },
  "id": 1
}
```

### Use Cases

The Solana getInflationGovernor method is essential for:

* Validators: Understanding how inflation affects staking rewards.
* Blockchain explorers: Displaying real-time inflation parameters.
* Web3 applications: Adjusting economic models based on inflation governance.
* Analytics platforms: Monitoring inflation trends and economic sustainability.

### Code Example – Web3 getInflationGovernor Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getInflationGovernor"
};

const fetchInflationGovernor = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      console.log("Inflation Governor:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getInflationGovernor error:", error.response?.data || error.message);
  }
};

fetchInflationGovernor();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getInflationGovernor API with Solana’s Core API to retrieve inflation governance data dynamically. By leveraging JSON-RPC parameters and endpoints, developers can ensure accurate tracking of inflation adjustments, staking rewards, and overall network economics.

\
