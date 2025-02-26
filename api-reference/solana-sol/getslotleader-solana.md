---
description: >-
  The getSlotLeader JSON-RPC method retrieves the current slot leader in the
  Solana blockchain. A slot leader is the validator responsible for producing
  blocks in the current slot.
---

# getSlotLeader – Solana

{% hint style="info" %}
The getSlotLeader RPC Solana method returns the node identity Pubkey of the validator currently leading the network’s block production.
{% endhint %}

Knowing the slot leader helps developers track block production, identify high-performing validators, and optimize network performance using Solana’s Core API.

This method supports an optional parameters object to customize the query with different commitment levels and minimum evaluation slots.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Optional Parameters

* object (optional): A configuration object containing:
  * commitment (string, optional): Defines the level of finality for the query.
  * minContextSlot (number, optional): The minimum slot at which the request can be evaluated.

### Result

The response returns a string representing the node identity Pubkey of the current slot leader, encoded in base-58.

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
    "method": "getSlotLeader"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the node identity Pubkey of the current slot leader.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": "ENvAW7JScgYq6o4zKZwewtkzzJgDzuJAFxYasvmEQdpS",
  "id": 1
}
```

### Error Handling

Common getSlotLeader error scenarios:

* Invalid parameters: If the provided parameters are incorrectly formatted.
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

The Solana getSlotLeader method is useful for:

* Validators and node operators: Identifying the current slot leader to monitor network leadership distribution.
* Blockchain explorers: Displaying the current slot leader for real-time network analysis.
* dApp developers: Tracking validator performance for strategic transaction planning.

### Code getSlotLeader Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getSlotLeader"
};

const fetchSlotLeader = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      console.log("Current Slot Leader:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getSlotLeader error:", error.response?.data || error.message);
  }
};

fetchSlotLeader();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getSlotLeader into Solana’s Core API, developers can track block production leadership, analyze transaction distribution, and optimize request performance. This JSON-RPC method is essential for applications that rely on real-time validator information and network monitoring.

\
