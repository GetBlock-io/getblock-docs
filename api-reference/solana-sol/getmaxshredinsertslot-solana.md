---
description: >-
  The getMaxShredInsertSlot JSON-RPC method retrieves the maximum slot observed
  after shred insertion in the Solana network.
---

# getMaxShredInsertSlot – Solana

{% hint style="success" %}
The getMaxShredInsertSlot RPC Solana method provides insight into the highest slot number observed after shred insert.&#x20;
{% endhint %}

This method is particularly useful for tracking data propagation and block finalization within the network.

Unlike other block or transaction retrieval methods, this RPC focuses on the point at which shreds (block data fragments) are fully inserted. Developers use this method to assess slot finalization efficiency, optimize node configurations, and troubleshoot data insertion issues in Core API implementations.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

The getMaxShredInsertSlot request does not require any parameters.

### Result

The response returns a single u64 value, representing the maximum slot seen after shred insertion.

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
    "method": "getMaxShredInsertSlot"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the highest slot number observed after shred insertion.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": 1234,
  "id": 1
}
```

### Error Handling

Common getMaxShredInsertSlot error scenarios:

* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.
* Invalid request format: Sending a malformed JSON request.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid request format"
  },
  "id": 1
}
```

### Use Cases

The Solana getMaxShredInsertSlot method is useful for:

* Validators and nodes: Monitoring the shred insert process for block finalization efficiency.
* Web3 analytics tools: Tracking real-time data insertion trends.
* Blockchain infrastructure: Understanding shred propagation trends to optimize network performance.

### Code getMaxShredInsertSlot Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getMaxShredInsertSlot"
};

const fetchMaxShredInsertSlot = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result !== undefined) {
      console.log("Max Shred Insert Slot:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getMaxShredInsertSlot error:", error.response?.data || error.message);
  }
};

fetchMaxShredInsertSlot();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getMaxShredInsertSlot into Solana’s Core API, developers can track shred insertion status, monitor block finalization, and improve transaction data flow. The JSON-RPC request structure allows seamless retrieval of slot insertion data, ensuring efficient blockchain operations for dApps, validators, and infrastructure providers.

\
