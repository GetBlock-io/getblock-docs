---
description: >-
  Invoke smart contract functions locally using eth_call for read-only queries
  without altering blockchain state. Essential for dApps to fetch data like
  balances or contract states via JSON-RPC.
---

# eth\_call - Ethereum

{% hint style="success" %}
The eth\_call method is used to invoke a smart contract function locally, without altering the blockchain's state.
{% endhint %}

The **eth\_call** method is essential in the Ethereum JSON-RPC interface, enabling read-only interaction with smart contracts on the Ethereum blockchain. It allows for querying data, such as balances and contract states, without modifying the blockchain, unlike state-changing methods like **eth\_sendTransaction**. As part of the Core API, **eth\_call** is critical for building Web3 dApps.

In Web3 apps, **metamask eth\_accounts** is commonly used to fetch a user's Ethereum accounts. If not connected, **eth\_requestAccounts** prompts users to connect their wallet. The difference between **eth\_accounts vs eth\_requestAccounts** is that the former retrieves the connected accounts, while the latter requests permission to access them. These methods ensure smooth wallet integration for dApps.

### Supported Networks

The **`eth_call` RPC Ethereum** method supports the following network types

* **Mainnet**
* **Testnet**: Sepolia, Hoodi

### Parameters

The **eth\_call** method requires two primary parameters:

1. **Transaction Call Object**:
   * **from**: (Optional) Ethereum address of the sender. Can be any address, used primarily for authorization purposes.
   * **to**: (Required) Ethereum address of the smart contract being called.
   * **gas**: (Optional) Gas limit for the call. If omitted, defaults to the Ethereum node's set limit.
   * **gasPrice**: (Optional) Price per unit of gas, used to calculate the transaction cost.
   * **value**: (Optional) Amount of Ether to send (usually `0x0` for read-only calls).
   * **data**: (Required) Encoded data containing the function signature and parameters to call on the smart contract.
2. **Block Parameter**:
   * Specifies the block context for the call. Can be set to:
     * **latest**: Latest block.
     * **earliest**: The genesis block.
     * **pending**: Pending block (before it's mined).
     * Specific block number or hash.

* **State Override Set** (Optional):\
  Allows temporary state modifications for the call without affecting the blockchain. It is an address-to-state mapping with the following fields:
  * **`balance`**: Override the balance of the specified address.
  * **`nonce`**: Override the transaction count of the specified address.
  * **`state`**: Override specific storage key-value pairs for the address.
  * **`code`**: Override the contract bytecode at the specified address.

### Request

#### URL

<pre class="language-json" data-full-width="false"><code class="lang-json"><strong>https://go.getblock.io/&#x3C;ACCESS-TOKEN>/
</strong></code></pre>

Here’s a sample cURL request using **eth\_call** to interact with a smart contract

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [
        {
            "to": "0x69498dd54bd25aa0c886cf1f8b8ae0856d55ff13",
            "value": "0x1"
        },
        "latest"
    ],
    "id": "getblock.io"
}'

```
{% endtab %}

{% tab title="wss" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
{"jsonrpc": "2.0",
"method": "eth_call",
"params": [{"to": "0x69498dd54bd25aa0c886cf1f8b8ae0856d55ff13", "value": "0x1"}, "latest"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful eth\_call response provides the requested data in hexadecimal format

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x"
}
```

### Body Params

The body parameters in a **eth\_call** request include:

* **jsonrpc**: Defines the JSON-RPC version (usually `"2.0"`).
* **method**: The method being called (`"eth_call"`).
* **params**: Array of parameters for the **eth\_call** method:
  1. **Transaction Call Object**: Contains all details of the transaction being called.
  2. **Block Parameter**: Specifies the block context in which to execute the call.

### Use Case

The **eth\_call** method is commonly used in **Web3** development to query data from Ethereum smart contracts. For example, it’s used to:

* **Fetch Token Balances**: By calling the `balanceOf(address)` function on ERC-20 tokens.
* **Read Contract States**: For instance, retrieving ownership information or any other state variable within the contract.
* **Simulate Transactions**: Developers use **eth\_call** to simulate function executions and check conditions before performing any state-changing operations like sending a transaction. This avoids unnecessary gas usage for failed transactions.

### Code Example

Below is a Python **`eth_call`** example using the requests library to send a JSON-RPC request for the **`eth_call`** method

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [
        {
            "to": "0x69498dd54bd25aa0c886cf1f8b8ae0856d55ff13",
            "value": "0x1"
        },
        "latest"
    ],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";

const headers = {
    "Content-Type": "application/json"
};

const payload = {
    jsonrpc: "2.0",
    method: "eth_call",
    params: [
        {
            to: "0x69498dd54bd25aa0c886cf1f8b8ae0856d55ff13", 
            value: "0x1" 
        },
        "latest"
    ],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("Result:", response.data.result);
        } else {
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        console.error("Request failed:", error.message);
    });

```
{% endtab %}
{% endtabs %}

This Python example demonstrates eth\_call usage to retrieve contract data in real-time, without altering Ethereum’s state. By using the eth\_call method with Web3 APIs, developers can access essential blockchain data seamlessly, integrating decentralized data access into applications while retaining control over transaction-free interactions.
