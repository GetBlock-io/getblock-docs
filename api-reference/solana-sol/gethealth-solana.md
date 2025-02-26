---
description: >-
  The getHealth JSON-RPC method retrieves the current health status of a Solana
  node.
---

# getHealth – Solana

{% hint style="success" %}
The getHealth RPC Solana method checks if a node is within HEALTH\_CHECK\_SLOT\_DISTANCE slots of the latest confirmed cluster slot. If the node is healthy, it returns an "ok" response; otherwise, it returns a JSON-RPC error.
{% endhint %}

This method is crucial for network monitoring, validator operations, and infrastructure management. If the node is behind, the error response may include additional details, such as the number of slots behind.

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
    "method": "getHealth"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful getHealth example response returns the health status of the node.

#### Healthy Response

```json
{
  "jsonrpc": "2.0",
  "result": "ok",
  "id": 1
}
Unhealthy Response (generic)
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32005,
    "message": "Node is unhealthy",
    "data": {}
  },
  "id": 1
}
```

#### Unhealthy Response (with additional information)

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32005,
    "message": "Node is behind by 42 slots",
    "data": {
      "numSlotsBehind": 42
    }
  },
  "id": 1
}
```

### Error Handling

Common getHealth error scenarios:

* Node out of sync: The node is lagging behind the latest cluster-confirmed slot.
* Network connectivity issues: The request fails due to API unavailability.
* Ambiguous method calls: In some cases, errors like the method getHealth() is ambiguous for the type player may occur in custom implementations, requiring method resolution.

### Use Cases

The Solana getHealth method is essential for:

* Validators: Monitoring node synchronization and performance.
* Blockchain explorers: Displaying node health status.
* Web3 applications: Ensuring nodes are in sync before submitting transactions.
* Infrastructure management: Detecting unhealthy nodes for maintenance and debugging.

### Code Example – Web3 getHealth Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getHealth"
};

const fetchNodeHealth = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result !== undefined) {
      console.log("Node Health Status:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getHealth error:", error.response?.data || error.message);
  }
};

fetchNodeHealth();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getHealth API with Solana’s Core API to monitor node health dynamically. By leveraging JSON-RPC parameters and endpoints, developers can ensure real-time tracking of node synchronization, detect unhealthy nodes, and optimize blockchain infrastructure for dApps and validators.

\
