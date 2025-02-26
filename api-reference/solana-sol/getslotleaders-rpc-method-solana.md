---
description: >-
  The getSlotLeaders JSON-RPC method retrieves the slot leaders for a specified
  slot range in the Solana blockchain.
---

# getSlotLeaders RPC Method – Solana

{% hint style="info" %}
The getSlotLeaders RPC Solana method provides an array of node identity public keys corresponding to the leaders for a given slot range.&#x20;
{% endhint %}

This information is crucial for understanding block production patterns and identifying potential network performance issues.

This method requires specifying a start slot and a limit to define the range. The limit can be set between 1 and 5,000, offering flexibility in tracking block leadership across various time intervals.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* u64 (optional): The start slot from which to retrieve the slot leaders.
* u64 (optional): The limit specifying how many slot leaders to return.
  * Must be between 1 and 5,000.

### Result

The response returns an array of strings, where each string is a node identity public key encoded in base-58. Each entry corresponds to a slot leader for a specific slot in the specified range.

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
    "method": "getSlotLeaders",
    "params": [100, 10]
}'
```
{% endtab %}
{% endtabs %}

In this example, the request starts from slot 100 and retrieves 10 slot leaders.

### Response

A successful request returns an array of slot leaders, starting from the specified start slot.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": [
    "ChorusmmK7i1AxXeiTtQgQZhQNiXYU84ULeaYF1EH15n",
    "ChorusmmK7i1AxXeiTtQgQZhQNiXYU84ULeaYF1EH15n",
    "ChorusmmK7i1AxXeiTtQgQZhQNiXYU84ULeaYF1EH15n",
    "ChorusmmK7i1AxXeiTtQgQZhQNiXYU84ULeaYF1EH15n",
    "Awes4Tr6TX8JDzEhCZY2QVNimT6iD1zWHzf1vNyGvpLM",
    "Awes4Tr6TX8JDzEhCZY2QVNimT6iD1zWHzf1vNyGvpLM",
    "Awes4Tr6TX8JDzEhCZY2QVNimT6iD1zWHzf1vNyGvpLM",
    "Awes4Tr6TX8JDzEhCZY2QVNimT6iD1zWHzf1vNyGvpLM",
    "DWvDTSh3qfn88UoQTEKRV2JnLt5jtJAVoiCo3ivtMwXP",
    "DWvDTSh3qfn88UoQTEKRV2JnLt5jtJAVoiCo3ivtMwXP"
  ],
  "id": 1
}
```

### Error Handling

Common getSlotLeaders error scenarios:

* Invalid parameters: If the start slot or limit are outside acceptable ranges.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.
* Malformed request: Incorrectly structured JSON requests.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid start slot or limit"
  },
  "id": 1
}
```

### Use Cases

The Solana getSlotLeaders method is useful for:

* dApp developers: Analyzing slot leader distribution for optimizing transaction performance.
* Web3 analytics tools: Tracking validator performance over time.
* Blockchain explorers: Displaying current and historical slot leaders.
* Validators and node operators: Monitoring block production patterns.

### Code getSlotLeaders Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getSlotLeaders",
  params: [100, 10]
};

const fetchSlotLeaders = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && Array.isArray(response.data.result)) {
      const leaders = response.data.result;
      if (leaders.length > 0) {
        leaders.forEach((leader, index) => {
          console.log(`Leader ${index + 1}: ${leader}`);
        });
      } else {
        console.log("No slot leaders found for the specified range.");
      }
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getSlotLeaders error:", error.response?.data || error.message);
  }
};

fetchSlotLeaders();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getSlotLeaders into Solana’s Core API, developers can track block production trends, monitor transaction performance, and analyze slot leadership across specified ranges. This JSON-RPC method plays a crucial role in maintaining network visibility and performance optimization.

\
