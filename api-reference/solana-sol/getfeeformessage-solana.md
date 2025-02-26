---
description: >-
  The getFeeForMessage JSON-RPC method retrieves the fee the Solana network will
  charge for processing a specific message. This method is a crucial part of the
  Solana getFeeForMessage API, enabling Web3
---

# getFeeForMessage - Solana

{% hint style="danger" %}
This method is only available in solana-core v1.9 or newer. For solana-core v1.8 and below, use the getFees method instead.
{% endhint %}

The getFeeForMessage method calculates the transaction fee for a given Base-64 encoded message at a specific blockhash. It helps developers estimate the cost of executing transactions before submission, ensuring better fee management and optimization within the Solana network.

### Supported Networks

This method is accessible through Solana API endpoints:

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* message (string): A Base-64 encoded transaction message.

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
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getFeeForMessage",
    "params": [
        "AQABAgIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEBAQAA",
        {
            "commitment": "processed"
        }
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful getFeeForMessage example response returns the fee associated with the given message at the specified blockhash.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": { "context": { "slot": 5068 }, "value": 5000 },
  "id": 1
}
```

### Error Handling

Common getFeeForMessage error scenarios:

* Invalid Base-64 message format: If the provided message is improperly encoded.
* Missing or expired blockhash: The blockhash used in the message has expired.
* Network connectivity issues: If the request fails due to API unavailability.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Blockhash not found"
  },
  "id": 1
}
```

### Use Cases

The Solana getFeeForMessage method is essential for:

* Wallet applications: Estimating transaction fees before signing.
* Smart contract interactions: Calculating transaction costs in Web3 getFeeForMessage applications.
* Blockchain explorers: Displaying estimated transaction fees for pending transactions.
* Developers: Ensuring users have sufficient SOL before submitting a transaction.

### Code Example – Web3 getFeeForMessage Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getFeeForMessage",
  params: [
    "AQABAgIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEBAQAA",
    { "commitment": "processed" }
  ]
};

const fetchTransactionFee = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      const fee = response.data.result.value;
      if (fee !== null) {
        console.log("Transaction Fee (lamports):", fee);
      } else {
        console.log("Transaction fee information is not available.");
      }
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getFeeForMessage error:", error.response?.data || error.message);
  }
};

fetchTransactionFee();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getFeeForMessage API with Solana’s Core API to retrieve real-time fee estimates. By leveraging JSON-RPC parameters and endpoints, developers can ensure accurate transaction fee calculations, helping users make informed decisions before broadcasting transactions.

\
