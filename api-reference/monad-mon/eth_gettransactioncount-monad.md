---
description: >-
  Example code for the eth_getTransactionCount JSON-RPC method. Complete guide
  on how to use eth_getTransactionCount JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getTransactionCount - Monad

This method returns the number of transactions sent from an address (the nonce).

## Parameters

| Parameter   | Type   | Required | Description                                                                   |
| ----------- | ------ | -------- | ----------------------------------------------------------------------------- |
| address     | string | Yes      | The address to get the transaction count for.                                 |
| blockNumber | string | No       | Block number in hex, or "latest", "earliest", "pending", "safe", "finalized". |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0x727752abbf7a9ef2f3c15e322dd43bf642d26eb7", "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="axios.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0x727752abbf7a9ef2f3c15e322dd43bf642d26eb7", "latest"],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0x727752abbf7a9ef2f3c15e322dd43bf642d26eb7", "latest"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "eth_getTransactionCount",
            "params": ["0x727752abbf7a9ef2f3c15e322dd43bf642d26eb7", "latest"],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1a"
}
```

## Response Parameters

| Field  | Type   | Description                                                      |
| ------ | ------ | ---------------------------------------------------------------- |
| result | string | The number of transactions sent from the address in hexadecimal. |

## Use Case

The `eth_getTransactionCount` method is essential for:

* Getting the correct nonce for new transactions
* Transaction building and signing
* Detecting pending transactions
* Wallet transaction management
* High-frequency trading nonce management
* Account activity analysis

## Error Handling

| Status Code | Error Message           | Cause                            |
| ----------- | ----------------------- | -------------------------------- |
| 403         | Forbidden               | Missing or invalid ACCESS-TOKEN. |
| -32602      | Invalid params          | Invalid address format.          |
| -32000      | Resource not found      | Block not found.                 |
| -32601      | Failed to parse request | syntax error                     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Get confirmed nonce
const nonce = await provider.getTransactionCount("0x727752abbf7a9ef2f3c15e322dd43bf642d26eb7");
console.log('Confirmed nonce:', nonce);

// Get pending nonce (includes pending transactions)
const pendingNonce = await provider.getTransactionCount(address, 'pending');
console.log('Pending nonce:', pendingNonce);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem" %}
```javascript
import { createPublicClient, http } from "viem";
import { monad } from "viem/chains";
/ Create Viem client with GetBlock
onst client = createPublicClient({
 chain: monad,
 transport: http(
   import.meta.env.MONAD_ACCESS_TOKEN
 ),
);
/ Using the method through Viem
sync function Call() {
 try {
   // Method-specific Viem implementation
   const result = await client.request({
     method: "eth_getTransactionCount",
     params: ["0x727752abbf7a9ef2f3c15e322dd43bf642d26eb7", "latest"],
   });
   console.log("Result:", result);
   return result;
 } catch (error) {
   console.error("Viem Error:", error);
   throw error;
 }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
