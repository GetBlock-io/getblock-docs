---
description: >-
  The getGenesisHash JSON-RPC method retrieves the genesis hash of the Solana
  blockchain.
---

# getGenesisHash – Solana

{% hint style="success" %}
The getGenesisHash RPC Solana method returns the base-58 encoded hash of the genesis block, which is the first block of the blockchain.
{% endhint %}

This hash uniquely identifies the blockchain instance, distinguishing between networks such as Mainnet and Devnet.

This method does not require any parameters and simply returns the genesis hash.

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
    "method": "getGenesisHash"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful getGenesisHash example response returns the base-58 encoded hash of the genesis block.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": "GH7ome3EiwEr7tu9JuTh2dpYWBJK3z69Xm1ZE3MEE6JC",
  "id": 1
}
```

### Error Handling

Common getGenesisHash error scenarios:

* Network connectivity issues: The request fails due to Solana API unavailability.
* Node synchronization issues: If the node is not fully synced, it may not return a valid genesis hash.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Genesis hash unavailable"
  },
  "id": 1
}
```

### Use Cases

The Solana getGenesisHash method is essential for:

* Blockchain explorers: Identifying the network by its unique genesis hash.
* Web3 applications: Ensuring connection to the correct chain.
* Validators and nodes: Verifying blockchain integrity before syncing.
* Analytics platforms: Tracking blockchain instances and differentiating between environments.

### Code Example – Web3 getGenesisHash Integration



{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getGenesisHash"
};

const fetchGenesisHash = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      console.log("Genesis Hash:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getGenesisHash error:", error.response?.data || error.message);
  }
};

fetchGenesisHash();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getGenesisHash API with Solana’s Core API to retrieve blockchain identity information dynamically. By leveraging JSON-RPC parameters and endpoints, developers can ensure correct network selection and chain validation, improving blockchain security and reliability.

\
