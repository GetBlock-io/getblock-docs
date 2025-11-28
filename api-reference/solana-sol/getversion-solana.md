---
description: >-
  The getVersion JSON-RPC method retrieves the current Solana version running on
  a node.
---

# getVersion – Solana

{% hint style="success" %}
The getVersion RPC Solana method provides the Solana core version and the feature set identifier.
{% endhint %}

It is essential for developers to verify the node's software version before making requests that depend on specific features.

### Supported Networks

This method is accessible through the following API endpoints:

* Mainnet

### Parameters

{% hint style="info" %}
This method does not require any parameters.
{% endhint %}

### Result

The response contains a JSON object with the following fields:

* `solana-core` (`string`): The software version of Solana Core.
* `feature-set` (`u32`): A unique identifier of the software's feature set.

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
    "method": "getVersion"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful request returns the Solana version and the feature set.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "feature-set": 2891131721,
    "solana-core": "1.16.7"
  },
  "id": 1
}
```

In this response:

* `solana-core`: The version is 1.16.7.
* `feature-set`: The unique feature set ID is 2891131721.

### Error Handling

Common getVersion error scenarios:

* **Network issues**: Connectivity problems with the Solana JSON-RPC API endpoints.
* **Invalid request format**: Incorrect JSON structure.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32600,
    "message": "Invalid request"
  },
  "id": 1
}
```

### Use Cases

The Solana getVersion method is useful for:

* **Web3 applications**: Ensuring compatibility with the Solana Core API.
* **Blockchain monitoring tools**: Displaying the node version.
* **Development environments**: Verifying the correct version of the node.

### Code getVersion Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getVersion"
};

const fetchSolanaVersion = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      console.log("Solana Node Version:");
      console.log(`  Version: ${response.data.result.solanaCore}`);
      if (response.data.result.featureSet !== undefined) {
        console.log(`  Feature Set: ${response.data.result.featureSet}`);
      }
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getVersion error:", error.response?.data || error.message);
  }
};

fetchSolanaVersion();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

By integrating Web3 getVersion into Solana’s Core API, developers can monitor node software versions, ensure compatibility, and debug API-related issues. This JSON-RPC method is essential for applications that depend on specific feature sets.

### Additional Notes

* Ensure compatibility with applications that use Java getVersion to avoid confusion.
* Always check for getVersion pending issues when interacting with newer API versions.

By following these guidelines, developers can effectively utilize the getVersion method to retrieve the Solana node version, monitor blockchain activity, and maintain compatibility with block, transaction, and value processing.
