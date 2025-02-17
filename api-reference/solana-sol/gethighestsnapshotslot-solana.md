---
description: >-
  The getHighestSnapshotSlot JSON-RPC method retrieves the highest slot
  information that the node has snapshots for.
---

# getHighestSnapshotSlot – Solana

{% hint style="danger" %}
This method is only available in solana-core v1.9 or newer. For solana-core v1.8 and below, use the getSnapshotSlot method instead.
{% endhint %}

The getHighestSnapshotSlot RPC Solana method returns the highest full snapshot slot and, if available, the highest incremental snapshot slot based on the full snapshot slot. This allows validators and nodes to determine the most recent snapshot they can use for ledger restoration and fast bootstrapping.

The response includes:

* full (u64): The highest full snapshot slot available.
* incremental (u64|null): The highest incremental snapshot slot based on the full snapshot slot.

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
curl --location "https://go.getblock.io/api-key" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getHighestSnapshotSlot"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful getHighestSnapshotSlot example response returns the highest snapshot slots available on the node.

#### Example Response (when a snapshot exists)

```json
{
  "jsonrpc": "2.0",
  "result": {
    "full": 100,
    "incremental": 110
  },
  "id": 1
}
```

#### Example Response (when no snapshot exists)

```json
{
  "jsonrpc": "2.0",
  "error": { "code": -32008, "message": "No snapshot" },
  "id": 1
}
```

### Error Handling

Common getHighestSnapshotSlot error scenarios:

* No snapshot available: The node does not currently have any snapshots.
* Network connectivity issues: The request fails due to API unavailability.
* Invalid node state: The node is not configured to store snapshots.

### Use Cases

The Solana getHighestSnapshotSlot method is essential for:

* Validators: Checking the latest available snapshot slots for fast synchronization.
* Blockchain explorers: Displaying snapshot data for node operators.
* Web3 applications: Ensuring dApps interact with up-to-date nodes.
* Infrastructure management: Monitoring snapshot availability for scaling node operations.

### Code Example – Web3 getHighestSnapshotSlot Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getHighestSnapshotSlot"
};

const fetchHighestSnapshotSlot = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      console.log("Highest Snapshot Slot:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getHighestSnapshotSlot error:", error.response?.data || error.message);
  }
};

fetchHighestSnapshotSlot();
```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getHighestSnapshotSlot API with Solana’s Core API to monitor snapshot availability dynamically. By leveraging JSON-RPC parameters and endpoints, developers can efficiently track snapshots, enabling optimized blockchain synchronization and reduced downtime for validators and nodes.
