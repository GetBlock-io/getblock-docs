---
description: >-
  The getRecentPerformanceSamples JSON-RPC method retrieves a list of recent
  performance samples, ordered in reverse slot order.
---

# getRecentPerformanceSamples – Solana

{% hint style="success" %}
The getRecentPerformanceSamples RPC Solana method enables developers to access recent performance data related to block and transaction processing.
{% endhint %}

Performance samples include information about the number of slots and transactions processed in a given time period, helping validators, developers, and network analysts gain insights into Solana’s efficiency.

The method supports an optional parameters field that allows users to specify the number of samples to retrieve, up to a maximum of 720. This flexibility makes it an essential tool for tracking performance trends within Solana’s Core API.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Optional Parameters

* limit (usize, optional): The number of recent performance samples to return.
  * Maximum: 720

### Result

The response returns an array of RpcPerfSample objects, each containing:

* slot (u64): The slot in which the sample was taken.
* numTransactions (u64): The number of transactions processed during the sample period.
* numSlots (u64): The number of slots completed during the sample period.
* samplePeriodSecs (u16): The duration of the sample window in seconds (typically 60 seconds).
* numNonVoteTransactions (u64): The number of non-vote transactions processed during the sample period. (Available in Solana v1.15 and later.)

To calculate the number of vote transactions, use the formula:

numTransactions - numNonVoteTransactions

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
    "method": "getRecentPerformanceSamples",
    "params": [4]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns an array of performance samples.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "numSlots": 126,
      "numTransactions": 126,
      "numNonVoteTransactions": 1,
      "samplePeriodSecs": 60,
      "slot": 348125
    },
    {
      "numSlots": 126,
      "numTransactions": 126,
      "numNonVoteTransactions": 1,
      "samplePeriodSecs": 60,
      "slot": 347999
    },
    {
      "numSlots": 125,
      "numTransactions": 125,
      "numNonVoteTransactions": 0,
      "samplePeriodSecs": 60,
      "slot": 347873
    },
    {
      "numSlots": 125,
      "numTransactions": 125,
      "numNonVoteTransactions": 0,
      "samplePeriodSecs": 60,
      "slot": 347748
    }
  ],
  "id": 1
}
```

### Error Handling

Common getRecentPerformanceSamples error scenarios:

* Invalid parameters: If an incorrect or out-of-range value is provided for limit.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.
* Malformed request: Incorrectly structured JSON requests.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid limit parameter"
  },
  "id": 1
}
```

### Use Cases

The Solana getRecentPerformanceSamples method is useful for:

* Validators and node operators: Monitoring transaction and slot processing performance.
* Blockchain explorers: Displaying real-time network statistics.
* dApp developers: Tracking performance trends to optimize transaction handling.
* Web3 analytics tools: Analyzing historical network efficiency and congestion.

### Code getRecentPerformanceSamples Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getRecentPerformanceSamples",
  params: [4]
};

const fetchRecentPerformanceSamples = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && Array.isArray(response.data.result)) {
      const samples = response.data.result;
      if (samples.length > 0) {
        samples.forEach((sample, index) => {
          console.log(`Sample ${index + 1}:`);
          console.log(`  Slot: ${sample.slot}`);
          console.log(`  Num Transactions: ${sample.numTransactions}`);
          console.log(`  Num Slots: ${sample.numSlots}`);
          console.log(`  Sample Period (seconds): ${sample.samplePeriodSecs}`);
        });
      } else {
        console.log("No performance samples found.");
      }
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getRecentPerformanceSamples error:", error.response?.data || error.message);
  }
};

fetchRecentPerformanceSamples();
```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getRecentPerformanceSamples into Solana’s Core API, developers can track block processing rates, monitor network congestion, and optimize transaction execution strategies. The JSON-RPC request provides real-time insights into Solana’s performance, making it a valuable tool for dApps, validators, and analytics platforms.

\
