---
description: >-
  The getMaxRetransmitSlot JSON-RPC method retrieves the maximum slot observed
  from the retransmit stage in the Solana network.
---

# getMaxRetransmitSlot – Solana

{% hint style="success" %}
The getMaxRetransmitSlot RPC Solana method provides insight into the highest slot number seen during the retransmit stage.&#x20;
{% endhint %}

This method is particularly useful for tracking network-wide slot dissemination and understanding how blocks and transactions are propagated within the network.

Unlike transaction or block retrieval methods, this RPC focuses on slot retransmission, which is vital for ensuring proper data flow in Solana’s Core API. Developers use this method to assess slot propagation delays, optimize node configurations, and troubleshoot data availability issues.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

{% hint style="info" %}
The getMaxRetransmitSlot request does not require any parameters.
{% endhint %}

### Result

The response returns a single u64 value, representing the maximum slot seen from the retransmit stage.

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
    "method": "getMaxRetransmitSlot"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the highest slot number observed from the retransmit stage.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": 1234,
  "id": 1
}
```

### Error Handling

Common getMaxRetransmitSlot error scenarios:

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

The Solana getMaxRetransmitSlot method is useful for:

* Validators and nodes: Monitoring the slot retransmit stage for block propagation efficiency.
* Web3 analytics tools: Tracking real-time slot dissemination.
* Blockchain infrastructure: Understanding slot propagation trends to optimize network performance.

### Code getMaxRetransmitSlot Example – Web3 Integration



{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getMaxRetransmitSlot"
};

const fetchMaxRetransmitSlot = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result !== undefined) {
      console.log("Max Retransmit Slot:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getMaxRetransmitSlot error:", error.response?.data || error.message);
  }
};

fetchMaxRetransmitSlot();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getMaxRetransmitSlot into Solana’s Core API, developers can track network slot propagation, monitor block dissemination, and improve transaction confirmation strategies. The JSON-RPC request structure allows seamless retrieval of retransmit slot data, ensuring efficient blockchain operations for dApps, validators, and infrastructure providers.

\
