---
description: >-
  The isBlockhashValid JSON-RPC method determines whether a given blockhash is
  still valid. This method is available in solana-core v1.9 and newer versions.
---

# isBlockhashValid – Solana

{% hint style="success" %}
The isBlockhashValid RPC Solana method helps developers verify if a blockhash remains valid for transaction processing.
{% endhint %}

In Web3 isBlockhashValid applications, this is crucial for ensuring transaction finality and avoiding errors related to expired blockhashes.

### Supported Networks

This method is accessible through the following API endpoints:

* Mainnet
* Devnet

### Parameters

#### Required Parameters

* string (required): The blockhash of the block to evaluate, provided as a base-58 encoded string.

#### Optional Parameters

* object (optional): A configuration object containing:
  * commitment (string): Specifies the level of finality for the request.
  * minContextSlot (number): The minimum slot that the request can be evaluated at.

### Result

The response returns a boolean indicating the validity of the specified blockhash.

#### Result Format

* true: The blockhash is still valid.
* false: The blockhash is no longer valid.

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
    "id": 45,
    "jsonrpc": "2.0",
    "method": "isBlockhashValid",
    "params": [
      "J7rBdM6AecPDEZp8aPq5iPSNKVkU5Q76F3oAV4eW5wsW",
      {"commitment": "processed"}
    ]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns a boolean value indicating blockhash validity.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 2483
    },
    "value": false
  },
  "id": 1
}
```

In this response:

* slot: The slot number at which the evaluation occurred.
* value: The validity of the specified blockhash.

### Error Handling

Common isBlockhashValid error scenarios:

* Invalid blockhash: If the provided blockhash is not found.
* Network issues: Connectivity problems with the Solana JSON-RPC API endpoints.
* Invalid parameters: Incorrect request structure.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid blockhash format"
  },
  "id": 1
}
```

### Use Cases

The Solana isBlockhashValid method is useful for:

* Transaction verification: Ensuring that a blockhash is still valid before submitting a transaction.
* Blockchain monitoring: Validating blockhashes across Web3 applications.
* API reliability: Reducing transaction failures due to expired blockhashes.

### Code isBlockhashValid Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key";
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 45,
  method: "isBlockhashValid",
  params: [
    "J7rBdM6AecPDEZp8aPq5iPSNKVkU5Q76F3oAV4eW5wsW",
    { commitment: "processed" }
  ]
};

const checkBlockhashValidity = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result !== undefined) {
      console.log("Blockhash Valid:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("isBlockhashValid error:", error.response?.data || error.message);
  }
};

checkBlockhashValidity();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 isBlockhashValid into Solana’s Core API, developers can ensure transaction reliability, optimize network performance, and reduce failed transactions due to expired blockhashes. This JSON-RPC method plays a critical role in transaction lifecycle management.

\
