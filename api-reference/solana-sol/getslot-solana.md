---
description: >-
  The getSlot JSON-RPC method retrieves the latest slot that has reached the
  given or default commitment level in the Solana blockchain.
---

# getSlot – Solana

{% hint style="info" %}
The getSlot RPC Solana method allows developers to retrieve the most recent slot number that has been confirmed based on the specified commitment level.
{% endhint %}

Slots represent points in time within Solana’s block production mechanism, making this method essential for real-time block and transaction monitoring in Solana’s Core API.

The method also supports an optional parameters object to refine results based on the commitment level and minimum evaluation slot.

### Supported Networks

This method is available on the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Optional Parameters

* object (optional): A configuration object containing:
  * commitment (string, optional): Defines the level of finality for the query.
  * minContextSlot (number, optional): The minimum slot at which the request can be evaluated.

### Result

The response returns a u64 value, representing the current slot number.

### Request Example

#### API Endpoints

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### cURL Example

{% tabs %}
{% tab title="curl" %}
```
curl --location "https://go.getblock.io/api-key" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getSlot"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the latest slot number.

#### Example Response

```
{
  "jsonrpc": "2.0",
  "result": 1234,
  "id": 1
}
```

### Error Handling

Common getSlot error scenarios:

* Invalid parameters: If an incorrect or malformed parameter is provided.
* Network errors: Connectivity issues with the Solana JSON-RPC API endpoints.

#### Example Error Response

```
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid parameter format"
  },
  "id": 1
}
```

### Use Cases

The Solana getSlot method is useful for:

* Validators and node operators: Tracking the latest slot to ensure synchronization.
* Blockchain explorers: Displaying real-time network slot data.
* dApp developers: Monitoring slot progression to optimize transaction timing.

### Code getSlot Example – Web3 Integration

{% tabs %}
{% tab title="curl" %}
```
const axios = require('axios');

const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };


const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getSlot"
};


const fetchCurrentSlot = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result !== undefined) {
      console.log("Current Slot:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getSlot error:", error.response?.data || error.message);
  }
};

fetchCurrentSlot();
```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getSlot into Solana’s Core API, developers can efficiently track block progress, monitor transaction confirmation times, and ensure real-time request handling. This JSON-RPC method is essential for building responsive blockchain applications and infrastructure services.

\
