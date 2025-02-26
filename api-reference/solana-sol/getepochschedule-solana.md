---
description: >-
  The getEpochSchedule JSON-RPC method retrieves the epoch schedule information
  from the Solana cluster's genesis configuration.
---

# getEpochSchedule – Solana

{% hint style="success" %}
The getEpochSchedule RPC Solana method provides essential details about the structure of epochs in the Solana network.
{% endhint %}

&#x20;It helps developers and validators understand the number of slots per epoch, leader schedule offsets, and how epochs evolve over time.

The response includes:

* slotsPerEpoch (u64): The maximum number of slots in each epoch.
* leaderScheduleSlotOffset (u64): Number of slots before an epoch starts to calculate its leader schedule.
* warmup (bool): Whether epochs start with fewer slots and increase over time.
* firstNormalEpoch (u64): The first epoch with a standard length.
* firstNormalSlot (u64): The first slot of the normal-length epoch.

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

```
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
    "method": "getEpochSchedule"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful getEpochSchedule example response provides information about epoch structure and scheduling.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "firstNormalEpoch": 8,
    "firstNormalSlot": 8160,
    "leaderScheduleSlotOffset": 8192,
    "slotsPerEpoch": 8192,
    "warmup": true
  },
  "id": 1
}
```

### Error Handling

Common getEpochSchedule error scenarios:

* Network issues: Request fails due to Solana API unavailability.
* Invalid response format: Unexpected JSON structure returned.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Epoch schedule data unavailable"
  },
  "id": 1
}
```

### Use Cases

The Solana getEpochSchedule method is essential for:

* Validators: Determining leader schedules and epoch transitions.
* Blockchain explorers: Displaying epoch duration and slot calculations.
* Web3 applications: Adjusting staking and governance models based on epoch structure.
* Analytics platforms: Understanding epoch-based network trends.

### Code Example – Web3 getEpochSchedule Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getEpochSchedule"
};

const fetchEpochSchedule = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      console.log("Epoch Schedule:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getEpochSchedule error:", error.response?.data || error.message);
  }
};

fetchEpochSchedule();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getEpochSchedule API with Solana’s Core API to retrieve real-time epoch structure details. By leveraging JSON-RPC parameters and endpoints, developers can track epoch progression, slot distribution, and leader schedule offsets, ensuring smooth blockchain operations.

\
