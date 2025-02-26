---
description: >-
  The getLeaderSchedule JSON-RPC method retrieves the leader schedule for a
  specific epoch in the Solana blockchain.
---

# getLeaderSchedule – Solana

{% hint style="success" %}
The getLeaderSchedule RPC Solana method provides the leader schedule for an epoch, mapping validator identities to their assigned slots. If no epoch is specified, the Solana getLeaderSchedule API returns the schedule for the current epoch.
{% endhint %}

This method can also be filtered by validator identity to obtain the schedule of a specific validator. If the requested epoch is not found, the response returns null. Otherwise, it returns a dictionary where each validator's public key maps to an array of slot indices.

### Supported Networks

Access this method via Solana API Endpoints:

* Mainnet
* Devnet

### Parameters

#### Optional Parameters

* epoch (u64, optional) – The epoch to retrieve the leader schedule for. If omitted, the current epoch is used.
* commitment (string, optional) – Commitment level of the request.
* identity (string, optional) – Validator identity (Base-58 encoded) to filter the response.

### getLeaderSchedule Example

This example demonstrates how to retrieve the getLeaderSchedule RPC Solana for a specific validator.

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
    "method": "getLeaderSchedule",
    "params": [
      null,
      {
        "identity": "4Qkev8aNZcqFNSRhQzwyLMFSsi94jHqE8WNVTJzTP99F"
      }
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

#### Successful Response Example

```json
{
  "jsonrpc": "2.0",
  "result": {
    "4Qkev8aNZcqFNSRhQzwyLMFSsi94jHqE8WNVTJzTP99F": [
      0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20,
      21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38,
      39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56,
      57, 58, 59, 60, 61, 62, 63
    ]
  },
  "id": 1
}
```

### Error Handling

Common getLeaderSchedule error scenarios:

* Invalid epoch number: If the epoch does not exist or is out of range.
* Network connectivity issues: The request fails due to API unavailability.
* Malformed request parameters: Incorrectly formatted request values.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Epoch data unavailable"
  },
  "id": 1
}
```

### Use Cases

The Solana getLeaderSchedule method is essential for:

* Validators: Predicting assigned slots for upcoming epochs.
* Blockchain explorers: Displaying leader schedules in real-time.
* Web3 applications: Ensuring efficient transaction scheduling.
* Analytics platforms: Tracking validator slot performance.

### Code getLeaderSchedule Example – Web3 Integration



{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getLeaderSchedule",
  params: [
    null,
    { "identity": "4Qkev8aNZcqFNSRhQzwyLMFSsi94jHqE8WNVTJzTP99F" }
  ]
};

const fetchLeaderSchedule = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      console.log("Leader Schedule:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getLeaderSchedule error:", error.response?.data || error.message);
  }
};

fetchLeaderSchedule();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the Web3 getLeaderSchedule API with Solana’s Core API to retrieve real-time leader schedules. By leveraging JSON-RPC parameters and endpoints, developers can optimize transaction execution and validator slot assignments, ensuring maximum efficiency in blockchain operations.

\
