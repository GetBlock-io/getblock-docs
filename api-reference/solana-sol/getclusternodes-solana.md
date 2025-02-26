---
description: >-
  The getClusterNodes JSON-RPC method retrieves information about all nodes
  participating in the Solana cluster.
---

# getClusterNodes – Solana

{% hint style="success" %}
The getClusterNodes RPC Solana method returns an array of JSON objects containing details about each active node in the cluster.&#x20;
{% endhint %}

The returned data includes the node’s public key, network addresses, software version, feature set, and shred version. This information is crucial for developers, validators, and network analysts to assess node distribution, connectivity, and operational details.

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
    "method": "getClusterNodes"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful getClusterNodes example response returns an array of objects detailing each node in the cluster.

The response includes:

* pubkey (string): Base-58 encoded node public key.
* gossip (string|null): Gossip network address of the node.
* tpu (string|null): TPU network address of the node.
* rpc (string|null): JSON-RPC network address (if the node has an active RPC service).
* version (string|null): Software version running on the node.
* featureSet (u32|null): Unique identifier for the node's feature set.
* shredVersion (u16|null): Shred version configured for the node.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "gossip": "10.239.6.48:8001",
      "pubkey": "9QzsJf7LPLj8GkXbYT3LFDKqsj2hHG7TA3xinJHu8epQ",
      "rpc": "10.239.6.48:8899",
      "tpu": "10.239.6.48:8856",
      "version": "1.0.0 c375ce1f"
    }
  ],
  "id": 1
}
```

### Error Handling

Common getClusterNodes error scenarios:

* Network connectivity issues: The request fails due to Solana RPC unavailability.
* Invalid request format: If the request is improperly structured.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Cluster data unavailable"
  },
  "id": 1
}
```

### Use Cases

The Solana getClusterNodes method is essential for:

* Network monitoring: Observing node connectivity and distribution.
* Validator tracking: Checking node software versions and infrastructure.
* Web3 applications: Ensuring proper RPC availability for dApps.
* Infrastructure analysis: Gathering metadata on the cluster’s performance and configuration.

### Code Example – Web3 getClusterNodes Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getClusterNodes"
};

const fetchClusterNodes = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      console.log("Cluster Nodes:", response.data.result);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getClusterNodes error:", error.response?.data || error.message);
  }
};

fetchClusterNodes();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getClusterNodes API with Solana’s Core API to dynamically retrieve real-time network data. By leveraging JSON-RPC parameters and endpoints, developers can gain deep insights into cluster activity, ensuring robust infrastructure for blockchain applications and validator operations.

\
