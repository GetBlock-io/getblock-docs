---
description: >-
  The isBlockhashValid JSON-RPC method checks whether a specific blockhash is
  still valid for submitting transactions on the Solana blockchain.
---

# isBlockhashValid - Solana

{% hint style="success" %}
The isBlockhashValid RPC Solana method verifies the validity of a blockhash by checking if it is still usable for transaction submission.
{% endhint %}

&#x20;As part of Solana’s Core API, it is critical for applications requiring real-time transaction validation, such as wallets or DeFi platforms.

This method supports optional parameters like commitment to specify the confirmation level of the blockhash. By ensuring that transactions reference valid blockhashes, developers prevent errors and optimize Web3 application performance.

#### Supported Networks

Access this method via Solana API Endpoints:

* Mainnet
* Devnet

#### Parameters

* blockhash (string, required):\
  The blockhash to validate, as a base-58 encoded string.
* commitment (object, optional):\
  Specifies the confirmation level for validating the blockhash. Supported options:
  * finalized (default): The blockhash is confirmed and immutable.
  * confirmed: The blockhash is confirmed but may still be reorganized.
  * processed: Not supported for this method.

#### Request

API Endpoints:

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Example (cURL) – isBlockhashValid example:**

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/api-key" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "isBlockhashValid",
    "params": [
        "J7rBdM6AecPDEZp8aPq5iPSNKVkU5Q76F3oAV4eW5wsW",
        { "commitment": "finalized" }
    ]
}'
```
{% endtab %}
{% endtabs %}

#### Response

A successful response returns a boolean indicating whether the blockhash is valid.

**Example Response:**

```json
{  
    "jsonrpc": "2.0",  
    "result": true,  
    "id": "getblock.io"  
}
```

#### Response Parameters:

* result: true if the blockhash is valid, false otherwise.

#### Error Handling

Common isBlockhashValid error scenarios include:

* Invalid blockhash format.
* Unsupported commitment level (e.g., processed).
* Missing or incorrect API key.

**Example Error Response:**

```json
{  
    "jsonrpc": "2.0",  
    "error": {  
        "code": -32602,  
        "message": "Invalid blockhash format"  
    },  
    "id": "getblock.io"  
}
```

#### Use Case

The isBlockhashValid RPC Solana method is ideal for:

* Wallets ensuring valid transaction submissions.
* dApps validating blockhash references before execution.
* DeFi platforms processing time-sensitive transactions.
* Explorers displaying the latest network state.

By verifying blockhash validity, developers prevent transaction failures and improve user experiences.

#### Code Example (JavaScript) – Web3 isBlockhashValid Integration:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');


const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };


const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "isBlockhashValid",
    params: [
        "J7rBdM6AecPDEZp8aPq5iPSNKVkU5Q76F3oAV4eW5wsW",
        { "commitment": "finalized" }
    ]
};


const checkBlockhashValidity = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            const isValid = response.data.result;
            console.log("Blockhash Valid:", isValid);
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

#### Integration with Web3

Integrate the isBlockhashValid RPC Solana method into Web3 applications to ensure valid transaction submissions and real-time network state tracking. By leveraging Core API parameters like commitment, developers optimize blockhash validation for wallets, dApps, and enterprise solutions.

\
