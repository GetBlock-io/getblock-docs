---
description: >-
  The getIdentity JSON-RPC method retrieves the identity public key of the
  current Solana node.
---

# getIdentity – Solana

{% hint style="success" %}
The getIdentity RPC Solana method returns the identity pubkey of the node as a base-58 encoded string.&#x20;
{% endhint %}

This information is essential for validators, blockchain explorers, and Web3 infrastructure providers to monitor and authenticate node connections.

This method does not require any parameters and simply returns the identity of the requesting node.

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
    "method": "getIdentity"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful getIdentity example response returns the identity pubkey of the node.

#### Example Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "identity": "2r1F4iWqVcb8M1DbAjQuFpebkQHY9hcVU4WuW2DJBppN"
  },
  "id": 1
}
```

### Error Handling

Common getIdentity error scenarios:

* Node misconfiguration: The node is not properly set up with an identity keypair.
* Network connectivity issues: The request fails due to API unavailability.
* zend\_auth::getinstance()->getidentity() error: This error can occur in PHP-based authentication systems and should be resolved by checking user session state.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Node identity unavailable"
  },
  "id": 1
}
```

### Use Cases

The Solana getIdentity method is essential for:

* Validators: Verifying their own identity and confirming their presence in the cluster.
* Blockchain explorers: Displaying node identity data for transparency.
* Web3 applications: Ensuring correct node authentication and connection.
* Network security: Checking whether a node is authorized to participate in the cluster.
* getIdentity for network Solana: Ensuring that nodes querying the blockchain are identified properly.

### Code Example – Web3 getIdentity Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getIdentity"
};

const fetchNodeIdentity = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result?.identity) {
      console.log("Node Identity:", response.data.result.identity);
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getIdentity error:", error.response?.data || error.message);
  }
};

fetchNodeIdentity();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the getIdentity API with Solana’s Core API to retrieve node identity information dynamically. By leveraging JSON-RPC parameters and endpoints, developers can ensure accurate authentication and network monitoring, supporting security and stability in blockchain infrastructure.

\
