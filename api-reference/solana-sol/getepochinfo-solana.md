---
description: >-
  The getEpochInfo JSON-RPC method retrieves information about the current epoch
  in the Solana blockchain.
---

# getEpochInfo – Solana

{% hint style="success" %}
The getEpochInfo RPC Solana method returns details about the current epoch, including slot index, total slots in the epoch, block height, and transaction count.&#x20;
{% endhint %}

This method helps developers track epoch progression, network state, and transaction activity.

The request accepts the following optional parameters:

* commitment (string): Specifies the finality level of the retrieved data.
* minContextSlot (number): Ensures the request is evaluated at or after the specified slot.

### Supported Networks

This method is accessible through Solana API endpoints:

* Mainnet
* Devnet

### Parameters

#### Optional Parameters

* commitment (string): Defines the finality level of the response.
* minContextSlot (number): The minimum slot at which the request should be evaluated.

### Request Example

#### API Endpoints

```
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
    "method": "getEpochInfo"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful getEpochInfo example response returns details about the current epoch, including the slot index and transaction count.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "absoluteSlot": 166598,
    "blockHeight": 166500,
    "epoch": 27,
    "slotIndex": 2790,
    "slotsInEpoch": 8192,
    "transactionCount": 22661093
  },
  "id": 1
}
```

### Error Handling

Common getEpochInfo error scenarios:

* Network issues: The request fails due to Solana API unavailability.
* Invalid parameters: Incorrect or missing configuration values.
* Outdated slot reference: The specified minContextSlot is too low.

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

The Solana getEpochInfo method is essential for:

* Blockchain explorers: Displaying real-time epoch status.
* Web3 applications: Tracking epoch changes for staking and governance.
* Validator nodes: Monitoring slot progression and transaction activity.
* Analytics platforms: Analyzing blockchain performance and transaction trends.

### Code Example – Web3 getEpochInfo Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getEpochInfo"
};


const fetchEpochInfo = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      console.log("Epoch Info:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getEpochInfo error:", error.response?.data || error.message);
  }
};

fetchEpochInfo();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getEpochInfo API with Solana’s Core API to retrieve up-to-date epoch details dynamically. By leveraging JSON-RPC parameters and endpoints, developers can ensure accurate tracking of block progression, transaction volume, and epoch-based calculations for staking and governance applications.

\
