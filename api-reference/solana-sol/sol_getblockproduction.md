---
description: >-
  The getBlockProduction method in Solana retrieves information about block
  production within a specified range of slots.
---

# getBlockProduction - Solana

{% hint style="success" %}
The getBlockProduction method in Solana allows you to retrieve block production data for specific slots or for an entire range of slots, depending on the parameters provided.
{% endhint %}

&#x20;This method is part of Solana’s Core API and is essential for tracking the block production across various validators. By querying this method, you can identify how many blocks each validator produced during the specified slot range. It is useful for monitoring validator performance and analyzing block production patterns in the Solana blockchain.

This method is available through the getBlockProduction RPC and allows you to filter results by validator identity, slot range, and commitment level, providing more granular insights into Solana’s block production activity.

### **Supported Networks**

The getBlockProduction RPC Solana method supports the following network types:

* Mainnet
* Devnet

### Parameters

Config (object) - Optional\
A configuration object containing the following optional fields:

* Commitment (optional): The level of commitment for the query. Options include:
  * processed: Returns the data for the most recent block.
  * finalized: Returns the data for finalized blocks.
  * confirmed: Returns the data for confirmed blocks.
  * If not provided, defaults to the current epoch.
* Range (object):
  * firstSlot (u64): The first slot to return block production information for (inclusive).
  * lastSlot (u64): The last slot to return block production information for (inclusive). If not provided, defaults to the highest slot.
* identity (string, optional): The validator identity (base-58 encoded) for which to return block production information.

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
    "method": "getBlockProduction",
    "params": [{
        "commitment": "finalized",
        "range": {
            "firstSlot": 100000,
            "lastSlot": 100100
        }
    }]
}'
```
{% endtab %}
{% endtabs %}

### Response

The response includes block production details by validator, showing how many blocks each validator produced in the specified range of slots.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "context": {
            "slot": 123033977
        },
        "value": {
            "byIdentity": {
                "12CUDzb3oe8RBQ4tYGqsuPsCbsVE4KWfktXRihXf8Ggq": [
                    84,
                    77
                ],
                "12ashmTiFStQ8RGUpi1BTCinJakVyDKWjRL6SWhnbxbT": [
                    176,
                    163
                ],
                "12oRmi8YDbqpkn326MdjwFeZ1bh3t7zVw8Nra2QK2SnR": [
                    84,
                    83
                ],
                "13442mSWLHjWwz3A39qjDEZzZR1jUWKNzgphNBsrS7xy": [
                    52,
                    51
                ],
                "138KHwTqKNWGLoo8fK5i8UxYtwoC5tC8o7M9rY1CDEjT": [
                    84,
                    81
                ],
                "13ftbVP7cpBp3JrQoXjfSWbNX8uS9ABLus4oZ4KaGodL": [
                    92,
                    91
                ]
            },
            "range": {
                "firstSlot": 122688000,
                "lastSlot": 123033977
            }
        }
    }
}
```

**Response Parameters**

* context.slot: The current slot at the time of the request.
* value.byIdentity: A dictionary of validator identities (base-58 encoded) with an array of two values:
  * The number of blocks produced in the first slot of the specified range.
  * The number of blocks produced in the last slot of the specified range.
* range.firstSlot: The first slot number in the range.
* range.lastSlot: The last slot number in the range.

### Use Case

The getBlockProduction method is useful in various scenarios, such as:

* Validator performance monitoring: Analyzing which validators are producing the most blocks during a given time period or slot range.
* Blockchain analytics: Identifying trends in block production, such as periods of low or high block production activity.
* Solana network health checks: Tracking whether block production is evenly distributed across validators and detecting potential issues with underperforming validators.

### Error Handling

Errors with the getBlockProduction method may occur under the following conditions:

* The provided API key is missing, expired, or invalid.
* The slot range or validator identity is not available or invalid.
* Other internal server issues may prevent the response from being returned.

### Example Error Response:

```json
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32007,
        "message": "Invalid slot range"
    },
    "id": "getblock.io"
}
```

### Code getBlockProduction Example - Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "getBlockProduction",
    params: [null]
};

const fetchBlockProduction = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            console.log("Block Production:", response.data.result || "No data available");
        } else {
            console.error("Unexpected status:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("Error:", error.response?.data || error.message);
    }
};

fetchBlockProduction();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

The Web3 getBlockProduction method is a powerful tool for developers building Web3 applications that need to track validator performance or analyze block production patterns. By utilizing this method, developers can easily access data about how many blocks each validator has produced within a specified slot range, making it a valuable resource for improving the efficiency and transparency of Solana’s blockchain.

