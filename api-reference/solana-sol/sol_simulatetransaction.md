---
description: >-
  The simulateTransaction JSON-RPC method simulates the execution of a
  transaction on the Solana blockchain without committing it to the ledger. This
  is essential for Web3 applications, such as wallets.
---

# simulateTransaction - Solana

{% hint style="success" %}
The simulateTransaction RPC method for Solana allows developers to test transactions before submission, ensuring they are valid and will execute as expected.
{% endhint %}

&#x20;As part of Solana’s Core API, it is crucial for applications requiring pre-execution validation, such as wallets or DeFi platforms.

This method supports optional parameters, including:

* sigVerify: To validate signatures.
* replaceRecentBlockhash: To update the blockhash.
* accounts: To retrieve account states.

By simulating transactions, developers can identify errors, optimize gas usage, and improve user experiences.

***

### Supported Networks

Access this method via the following Solana API endpoints:

* Mainnet
* Devnet

***

### Parameters

#### transaction (string, required)

The transaction to simulate, encoded as a base58 or base64 string. The transaction must include a valid blockhash but does not need to be signed.

#### Config (object, optional)

Customize the simulation with the following fields:

* sigVerify (bool, optional): Verify transaction signatures (default: false). Conflicts with replaceRecentBlockhash.
* commitment (string, optional): Confirmation level: finalized (default), confirmed, or processed.
* encoding (string, optional): Encoding for transaction data: base58 (deprecated) or base64 (recommended).
* replaceRecentBlockhash (bool, optional): Replace the transaction’s blockhash with the latest one (default: false). Conflicts with sigVerify.
* accounts (object, optional): Retrieve account states with the following fields:
*
  * encoding: Data encoding: base64, base64+zstd, or jsonParsed.
  * addresses: Array of account addresses (base-58 encoded).

***

### Request

API Endpoints:

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Example (cURL)

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/api-key" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "simulateTransaction",
    "params": [
        "4hXTCkRzt9WyecNzV1XPgCDfGAZzQKNxLXgynz5QDuWWPSAZBZSHptvWRL3BjCvzUXRdKvHL2b7yGrRQcWyaqsaBCncVG7BFggS8w9snUts67BSh3EqKpXLUm5UMHfD7ZBe9GhARjbNQMLJ1QD3Spr6oMTBU6EhdB4RD8CP2xUxr2u3d6fos36PD98XS6oX8TQjLpsMwncs5DAMiD4nNnR8NBfyghGCWvCVifVwvA8B8TJxE1aiyiv2L429BCWfyzAme5sZW8rDb14NeCQHhZbtNqfXhcp2tAnaAT",
        { "encoding": "base64", "sigVerify": true }
    ]
}'
```
{% endtab %}
{% endtabs %}

***

### Response

A successful response includes simulation results, such as execution logs, errors, and account states.

Example Response:

```json

{  
    "id": "getblock.io",  
    "jsonrpc": "2.0",  
    "result": {  
        "context": { "slot": 123032941 },  
        "value": {  
            "accounts": null,  
            "err": "BlockhashNotFound",  
            "logs": []  
        }  
    }  
}  
```

#### Key Response Fields:

* context.slot: The slot at which the simulation was executed.
* value.err: Error message if the transaction fails.
* value.logs: Execution logs for debugging.
* value.accounts: Account states (if requested).

***

### Error Handling

Common error scenarios with simulateTransaction include:

* Invalid transaction encoding or blockhash.
* Signature verification failures (if sigVerify is enabled).
* Missing or incorrect API key.

Example Error Response:

```json
{  
    "jsonrpc": "2.0",  
    "error": {  
        "code": -32602,  
        "message": "Invalid transaction encoding"  
    },  
    "id": "getblock.io"  
} 
```

***

### Use Case

The simulateTransaction RPC method is ideal for:

* Wallets validating transactions before submission.
* dApps debugging failed transactions.
* DeFi platforms estimating gas costs and outcomes.
* Explorers analyzing transaction execution paths.

By simulating transactions, developers can reduce errors, optimize costs, and enhance user trust.

***

### Code simulateTransaction Example  – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');


const url = "https://go.getblock.io/api-key"; 
const headers = { "Content-Type": "application/json" };


const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "simulateTransaction",
    params: [
        "4hXTCkRzt9WyecNzV1XPgCDfGAZzQKNxLXgynz5QDuWWPSAZBZSHptvWRL3BjCvzUXRdKvHL2b7yGrRQcWyaqsaBCncVG7BFggS8w9snUts67BSh3EqKpXLUm5UMHfD7ZBe9GhARjbNQMLJ1QD3Spr6oMTBU6EhdB4RD8CP2xUxr2u3d6fos36PD98XS6oX8TQjLpsMwncs5DAMiD4nNnR8NBfyghGCWvCVifVwvA8B8TJxE1aiyiv2L429BCWfyzAme5sZW8rDb14NeCQHhZbtNqfXhcp2tAnaAT",
        { encoding: "base64", sigVerify: true }
    ]
};


const simulateTransaction = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200 && response.data.result?.value) {
            const simulationResult = response.data.result.value;

            if (simulationResult.err) {
                console.error("Transaction Error:", simulationResult.err);
            } else {
                console.log("Simulation Logs:", simulationResult.logs);
            }
        } else {
            console.error("Unexpected response:", response.data);
        }
    } catch (error) {
        console.error("simulateTransaction error:", error.response?.data || error.message);
    }
};

simulateTransaction();

```
{% endtab %}
{% endtabs %}

***

### Integration with Web3

Integrate the Web3 simulateTransaction RPC method into Web3 applications to validate and debug transactions before submission. By leveraging Core API parameters like sigVerify and accounts, developers ensure reliable and efficient transaction processing for wallets, dApps, and enterprise solutions.

\
