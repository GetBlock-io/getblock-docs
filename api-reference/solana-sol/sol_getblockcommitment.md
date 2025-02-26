---
description: >-
  The getBlockCommitment method retrieves commitment and stake information for a
  specific block by its slot number. It’s essential for tracking block
  validation and analyzing Solana network consensus.
---

# getBlockCommitment - Solana

{% hint style="success" %}
The getBlockCommitment RPC Solana method allows you to query the commitment level and total stake associated with a specific block
{% endhint %}

&#x20;By using the block's slot as a parameter, this method provides valuable insights into the state of a block in terms of its commitment and the total stake at the time of the block’s finalization. It is useful for applications that need to monitor block validation progress or analyze Solana network consensus. The getBlockCommitment method is part of Solana's Core API and can be accessed through GetBlock.io’s Web3 getBlockCommitment service.

### **Supported Networks**

The getBlockCommitment RPC Solana method supports the following network types:

* Mainnet
* Devnet

### Parameters

* block (u64, required): The slot number (u64) identifying the block whose commitment information is being requested.

### Request

URL(Endpoints)

<pre class="language-json" data-full-width="false"><code class="lang-json"><strong>https://go.getblock.io/&#x3C;ACCESS-TOKEN>/
</strong></code></pre>

### Example (cURL):

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getBlockCommitment",
    "params": [122791192]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response provides the commitment information for the requested block. If there is an issue retrieving the block data, the response may include an error message.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "commitment": null,
        "totalStake": 388997198076741924
    }
}
```

**Response Parameters**

* id: A unique request identifier, matching the ID sent in the request body.
* jsonrpc: Specifies the use of JSON-RPC version 2.0.
* result:
  * commitment: The commitment level of the block (may be null if not available).
  * totalStake: The total stake for the block, represented as a large integer.

### Use Case

The getBlockCommitment method is essential for applications needing to track the commitment status of blocks in the Solana network. For example, block explorers can use this method to analyze the validation status of blocks, while other Web3 applications might use it to track transaction finalization or staking information associated with blocks. By using this method, developers can better understand the behavior of transactions and blocks in the Solana network.

### Code getBlockCommitment Example - Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "getBlockCommitment",
    params: [122791192]
};

const fetchBlockCommitment = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            const blockCommitment = response.data.result;
            console.log("Block Commitment:", blockCommitment || "No data available");
        } else {
            console.error("Unexpected status:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("Error:", error.response?.data || error.message);
    }
};

fetchBlockCommitment();
```
{% endtab %}
{% endtabs %}

This getBlockCommitment example demonstrates how to query the commitment information for a specific block using the getBlockCommitment method, offering insights into its validation status and associated stake.

### Integration with Web3

The Web3 getBlockCommitment method is integral to the Solana Core API, providing detailed commitment and stake data for blocks. By integrating this method into your Web3 applications, you can easily retrieve block-related information, such as transaction finalization and network commitment. This method enhances the functionality of explorers, wallets, and other blockchain-based applications that rely on accurate and real-time block data.
