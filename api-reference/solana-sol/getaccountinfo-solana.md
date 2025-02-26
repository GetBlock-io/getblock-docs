---
description: >-
  The getAccountInfo JSON-RPC method retrieves detailed information about a
  specific account on the Solana blockchain, including its balance, owner, data,
  and metadata.
---

# getAccountInfo - Solana

{% hint style="success" %}
The getAccountInfo RPC Solana method returns all metadata associated with an account, such as lamports (balance), owner, data, and rentEpoch.
{% endhint %}

&#x20;As part of Solana’s Core API, it is critical for applications requiring real-time account state analysis, such as Web3 wallets or decentralized exchanges.

This method supports optional parameters like encoding (to specify the data format) and dataSlice (to retrieve partial account data). It is a foundational tool for building blockchain-based applications that interact with on-chain accounts.

#### Supported Networks

Access this method via Solana API Endpoints:

* Mainnet
* Devnet

#### Parameters

Pubkey (string, required):\
The account’s public key as a base-58 encoded string.

Config (object, optional):\
Customize the request with the following fields:

* commitment (string, optional):\
  Confirmation level: finalized (default), confirmed, or processed.
* encoding (string, optional):\
  Data encoding format: base58, base64, base64+zstd, or jsonParsed.
* dataSlice (object, optional):\
  Retrieve a subset of account data using offset (starting byte) and length (number of bytes).
* minContextSlot (number, optional):\
  The minimum slot at which the account data should be evaluated.

#### Request

API Endpoints:

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Example (cURL) – getAccountInfo example:**

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getAccountInfo",
    "params": [
        "vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg",
        {"encoding": "base58"}
    ]
}'
```
{% endtab %}
{% endtabs %}

#### Response

A successful response returns account metadata, including balance, owner, and data. If the account doesn’t exist, value is null.

**Example Response:**

```json
{  
    "jsonrpc": "2.0",  
    "result": {  
        "context": { "apiVersion": "2.0.15", "slot": 341197053 },  
        "value": {  
            "data": ["", "base58"],  
            "executable": false,  
            "lamports": 88849814690250,  
            "owner": "11111111111111111111111111111111",  
            "rentEpoch": 18446744073709551615,  
            "space": 0  
        }  
    },  
    "id": 1  
}
```

#### Key Response Fields:

* lamports: Balance in lamports.
* owner: Program ID owning the account.
* data: Encoded account data or parsed JSON (if jsonParsed encoding is used).
* executable: Whether the account contains a program.
* rentEpoch: Epoch at which rent is due.
* space: Size of account data in bytes.

#### Error Handling

Common getAccountInfo error scenarios include:

* Invalid Pubkey format.
* Unsupported encoding or dataSlice configuration.
* Missing or incorrect API key.

**Error Response Example:**

```json
{  
    "jsonrpc": "2.0",  
    "error": {  
        "code": -32602,  
        "message": "Invalid pubkey"  
    },  
    "id": 1  
}
```

#### Use Case

The getAccountInfo RPC Solana method is ideal for:

* Wallets displaying account details and balances.
* Explorers analyzing account states and program interactions.
* DeFi platforms verifying account ownership and data.
* Analytics tools tracking account activity and value changes.

By using jsonParsed encoding, developers simplify parsing program-specific account data, enhancing Web3 integrations.

#### Code Example (JavaScript) – Web3 getAccountInfo Integration 

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');


const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };


const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "getAccountInfo",
    params: [
        "vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg",
        { encoding: "jsonParsed" }
    ]
};


const fetchAccountInfo = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            const accountInfo = response.data.result?.value;
            if (accountInfo) {
                console.log("Account Balance (lamports):", accountInfo.lamports);
                console.log("Account Owner:", accountInfo.owner);
            } else {
                console.log("Account not found.");
            }
        } else {
            console.error("Unexpected response:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("getAccountInfo error:", error.response?.data || error.message);
    }
};

fetchAccountInfo();

```
{% endtab %}
{% endtabs %}

#### Integration with Web3

Integrate the getAccountInfo RPC Solana method into Web3 applications to enable real-time account state tracking and analysis. By leveraging Core API parameters like encoding and dataSlice, developers can retrieve precise account data for wallets, dApps, and enterprise solutions.

\
