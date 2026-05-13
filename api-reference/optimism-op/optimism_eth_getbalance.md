---
description: >-
  Example code for the eth_getBalance json-rpc method. Сomplete guide on how to
  use eth_getBalance json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBalance - Optimism

This method retrieves the native ETH balance of any address on the Optimism network. The balance is returned in wei (the smallest unit of ETH, where 1 ETH = 10^18 wei). You can query the balance at a specific block height or use tags like 'latest', 'earliest', or 'pending'. This is essential for wallet applications, balance verification, and transaction validation.

## Parameters

| Parameter | Type           | Description                                              | Required |
| --------- | -------------- | -------------------------------------------------------- | -------- |
| address   | DATA, 20 Bytes | The address to check the balance of.                     | Yes      |
| block     | QUANTITY\|TAG  | Block number in hex, or 'latest', 'earliest', 'pending'. | Yes      |

## Request Sample

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["0xb8b2522480f850eb198ada5c3f31ac528538d2f5", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Javascript(Axios)" %}
```bash
import axios from 'axios'
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "eth_getBalance",
  "id": "getblock.io",
  "params": [
        "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
        "latest"
    ],
};

let config = {
  method: "post",
  maxBodyLength: Infinity,
  url: "https://go.getblock.us/<ACCESS_TOKEN>",
  headers: {
    "Content-Type": "application/json",
  },
  data: data,
};

axios
  .request(config)
  .then((response) => {
    console.log(JSON.stringify(response.data));
  })
  .catch((error) => {
    console.log(error);
  });

```
{% endtab %}

{% tab title="Python(Request)" %}
{% code overflow="wrap" %}
```py
import requests
import json

url = "https://go.getblock.us/<ACESS_TOKEN>"

payload = json.dumps({
  "jsonrpc": "2.0",
  "method": "eth_getBalance",
  "id": "getblock.io",
  "params": [
        "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
        "latest"
    ],
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rs
import requests
import json

url = "https://go.getblock.us/<ACESS_TOKEN>"

payload = json.dumps({
  "jsonrpc": "2.0",
  "method": "eth_getBalance",
  "id": "getblock.io",
  "params": [
        "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
        "latest"
    ],
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

A successful response returns the following:

```json
 {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x0"
}
```

## Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The balance in wei as a hexadecimal string. `0x1bc16d674ec80000` equals 2 ETH (2000000000000000000 wei).

## Use Case

The eth\_getBalance method is commonly used for:

* **Wallet balance display**
* **Transaction validation**
* **Account monitoring**
* **Portfolio tracking**

#### Error handling

| Status Code | Error Message    | Cause                                       |
| ----------- | ---------------- | ------------------------------------------- |
| 404         | Not found        | Missing or invalid ACCESS\_TOKEN.           |
| -32602      | Invalid argument | Wallet address isn't accurate or incomplete |

### Integration with Web3

The `eth_getBalance` can help developers:

* Fetch balances dynamically without requiring a transaction
* Power trustless dashboards and explorers
* Validate user accounts before executing transactions
* Track portfolio values in real time
* Access historical balances using specific block numbers

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getBalance", ["0xb8b2522480f850eb198ada5c3f31ac528538d2f5", "latest"]);
    console.log("The result:", result);
    return result;
  } catch (error) {
    console.error("The error:", error);
    throw error;
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```jsx
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: optimism,
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'),
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
            method: 'eth_getBalance',
            params: ["0xb8b2522480f850eb198ada5c3f31ac528538d2f5", "latest"],
        });
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }
}

```
{% endtab %}
{% endtabs %}
