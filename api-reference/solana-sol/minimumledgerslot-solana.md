---
description: >-
  The minimumLedgerSlot JSON-RPC method retrieves the lowest slot that a Solana
  node has information about in its ledger.
---

# minimumLedgerSlot – Solana

{% hint style="success" %}
The minimumLedgerSlot RPC Solana method is used in Web3 minimumLedgerSlot applications to determine the starting point of available ledger data.&#x20;
{% endhint %}

This is essential for tasks that require historical transaction data.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

{% hint style="info" %}
This method does not require any parameters.
{% endhint %}

### Result

The response returns a u64 integer indicating the minimum ledger slot that the Solana node retains in its ledger.

#### Result Format

* u64: The lowest slot number in the ledger.

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
    "method": "minimumLedgerSlot"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the minimum ledger slot.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": 1234,
  "id": 1
}
```

In this response:

* result: The minimum ledger slot is 1234.

### Error Handling

Common minimumLedgerSlot error scenarios:

* Network issues: Connectivity problems with the Solana JSON-RPC API endpoints.
* Invalid request format: Incorrect JSON structure.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32600,
    "message": "Invalid request"
  },
  "id": 1
}
```

### Use Cases

The Solana minimumLedgerSlot method is useful for:

* Historical data retrieval: Determining the earliest available slot.
* Blockchain synchronization: Aligning data queries with available ledger history.
* Analytics tools: Calculating ledger size and data retention periods.

### Code minimumLedgerSlot Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "minimumLedgerSlot"
};

const fetchMinimumLedgerSlot = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result !== undefined) {
      console.log("Minimum Ledger Slot:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("minimumLedgerSlot error:", error.response?.data || error.message);
  }
};

fetchMinimumLedgerSlot();
```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 minimumLedgerSlot into Solana’s Core API, developers can accurately track ledger history, analyze block availability, and ensure compatibility with blockchain applications that depend on historical data.
