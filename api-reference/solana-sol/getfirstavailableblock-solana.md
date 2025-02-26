---
description: >-
  Retrieve the first available block in Solana's blockchain for precise
  synchronization and data analysis. No parameters required.
---

# getFirstAvailableBlock - Solana

{% hint style="info" %}
The getFirstAvailableBlock method in Solana provides the first available block from the blockchain, allowing developers to retrieve the earliest block that can be queried.&#x20;
{% endhint %}

The getFirstAvailableBlock method provides information about the earliest available block in the Solana blockchain. It is used for data synchronization and analysis, allowing applications to start processing from the first accessible block. This method is part of Solana's Core API and ensures the proper operation of services that rely on historical blockchain data.

### Supported Networks

Access this method via Solana API Endpoints:

* Mainnet
* Devnet

### Parameters

{% hint style="info" %}
No parameters are required for this method.
{% endhint %}

## Request

#### Endpoint URL:&#x20;

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### Example Request (cURL):

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getFirstAvailableBlock",
    "params": []
}'
```
{% endtab %}
{% endtabs %}

### Response

The response contains the block number of the first available block.

#### Example Response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": 122257143
}
```

#### Response Parameters:

* id: A unique identifier for the request, matching the ID sent in the request body.
* jsonrpc: Specifies the use of JSON-RPC version 2.0.
* result: The block number of the first available block in the blockchain.

### Use Cases

The getFirstAvailableBlock method is valuable in a range of applications, including:

* Blockchain synchronization: Ensuring that your application starts from the earliest block available.
* Transaction monitoring: Useful in blockchain explorers or analytics tools that need to start querying blocks from the earliest point.
* API integration: Developers can use this method to ensure data integrity and consistency by retrieving the first available block and tracking the blockchain from there.

#### Error Handling

Errors with the getFirstAvailableBlock method may occur in the following scenarios:

* The API key is missing, expired, or invalid.
* An issue with the endpoint or network causes the request to fail.

Example Error Response:

```json
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32007,
        "message": "No available blocks"
    },
    "id": "getblock.io"
}
```

### Code getFirstAvailableBlock Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "getFirstAvailableBlock",
    params: []
};

const fetchFirstAvailableBlock = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200 && response.data.result !== undefined) {
            const firstAvailableBlock = response.data.result;
            console.log("First Available Block:", firstAvailableBlock);
        } else {
            console.error("Unexpected response:", response.data);
        }
    } catch (error) {
        console.error("getFirstAvailableBlock error:", error.response?.data || error.message);
    }
};

fetchFirstAvailableBlock();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

The Web3 getFirstAvailableBlock method is a useful tool for Web3 applications that need to query data starting from the first available block. By utilizing this method, developers can ensure their applications handle block data consistently and without gaps, making it essential for projects that require precise synchronization with Solana’s blockchain.

\
