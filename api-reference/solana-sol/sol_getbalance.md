---
description: >-
  The getBalance method retrieves the balance of a Solana account in lamports
  (the smallest unit of SOL). It’s essential for wallets, explorers, and
  analytics tools to fetch real-time balance data.Examp
---

# getBalance - Solana

{% hint style="success" %}
The getBalance method in Solana allows you to retrieve the balance of an account in lamports, which is the smallest unit of Solana’s native token (SOL).
{% endhint %}

This method is particularly useful for applications that need to display a user’s account balance or monitor changes in funds, such as wallets or exchange platforms. The balance can be queried using a base-58 encoded public key of the account, and the result will include the account’s current balance in lamports.

The method is part of the Solana Core API and can be accessed through the Solana JSON-RPC interface. Developers can customize their requests by specifying the level of commitment, which influences the level of transaction confirmation in the blockchain state

### **Supported Networks**

The getBalance RPC Solana method supports the following network types:

* Mainnet
* Devnet

### Parameters

* Pubkey (string, required): The base-58 encoded public key of the account to query. This parameter is required to specify the account whose balance is being retrieved.
* Commitment (object, optional): Defines the level of commitment required for the requested transaction data. Possible values include:
  * "processed": the most recent block data, not finalized.
  * "confirmed": a more reliable but potentially slower confirmation level.
  * "finalized": the highest level of confirmation, suitable for finalizing transactions.

### Request

URL(Endpoints)

<pre class="language-json" data-full-width="false"><code class="lang-json"><strong>https://go.getblock.io/&#x3C;ACCESS-TOKEN>/
</strong></code></pre>

### Example (cURL):

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/api-key" -XPOST \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getBalance",
    "params": ["83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri", null]
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response will contain the account’s balance and additional context information. The response will include the current slot number and the balance in lamports.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "context": {
            "slot": 123033162
        },
        "value": 0
    }
}
```

**Response Parameters**

* id: A unique request identifier, matching the ID sent in the request body.
* jsonrpc: Specifies the use of JSON-RPC version 2.0.
* result:
  * context:
    * slot: The current slot number for the requested balance.
  * value: The account balance in lamports.

### Use Case

The getBalance method is highly useful in Web3 development, particularly for applications that require real-time information about an account’s balance. It can be used in wallet applications to show the user’s current balance, in block explorers to display account details, or by developers analyzing the state of a Solana account for transaction processing.

### Code getBalance Example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/YOUR-API-KEY";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    id: 1, 
    method: "getBalance",
    params: [
        "83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri",
        null
    ]
};

const fetchBalance = async () => {
    try {
        const response = await axios.post(url, payload, { headers });

        if (response.status === 200) {
            const balanceInfo = response.data.result;
            console.log("Account Balance:", balanceInfo?.value || "No balance data available");
        } else {
            console.error("Unexpected status:", response.status, response.statusText);
        }
    } catch (error) {
        console.error("Error:", error.response?.data || error.message);
    }
};

fetchBalance();
```
{% endtab %}
{% endtabs %}

This example demonstrates how to query an account's balance programmatically using the getBalance method. The returned balance is in lamports, and developers can adjust the encoding and other optional parameters to suit their needs.

### Integration with Web3

The Web3 getBalance method is an essential tool for interacting with the Solana blockchain through the JSON-RPC interface. By using this method, developers can easily fetch real-time balance information for accounts on Solana, which is vital for building decentralized applications, wallets, and other Web3 tools. The flexible request parameters allow for tailored queries, ensuring that developers can balance the need for accuracy and performance.
