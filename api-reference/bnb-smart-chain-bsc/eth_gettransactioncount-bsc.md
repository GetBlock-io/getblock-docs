---
description: >-
  Example code for the eth_getTransactionCount JSON RPC method. Сomplete guide
  on how to use eth_getTransactionCount JSON RPC in GetBlock Web3 documentation.
---

# eth\_getTransactionCount - BSC

This method returns the number of transactions sent from an address (nonce) on the BNB Smart Chain. This is essential for creating new transactions, as each transaction requires the correct nonce.

## Parameters

| Parameter   | Type   | Required | Description                         |
| ----------- | ------ | -------- | ----------------------------------- |
| address     | string | Yes      | Address to get nonce for            |
| blockNumber | string | Yes      | Block number or "latest", "pending" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0x8D97689C9818892B700e27F316cc3E41e17fBeb9", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getTransactionCount',
    params: ['0x8D97689C9818892B700e27F316cc3E41e17fBeb9', 'latest'],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const nonce = parseInt(response.data.result, 16);
    console.log('Nonce:', nonce);
})
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0x8D97689C9818892B700e27F316cc3E41e17fBeb9", "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
nonce = int(response.json()["result"], 16)
print(f"Nonce: {nonce}")
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_getTransactionCount",
        "params": ["0x8D97689C9818892B700e27F316cc3E41e17fBeb9", "latest"],
        "id": "getblock.io"
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x15"
}
```

## Response Parameters

| Parameter | Type   | Description              |
| --------- | ------ | ------------------------ |
| result    | string | Nonce in hex (0x15 = 21) |

## Use Cases

* Get nonce for transaction signing
* Track account activity
* Detect pending transactions
* Prevent nonce conflicts

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const nonce = await provider.getTransactionCount('0x8D97689C9818892B700e27F316cc3E41e17fBeb9');
console.log('Nonce:', nonce);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const nonce = await client.getTransactionCount({
    address: '0x8D97689C9818892B700e27F316cc3E41e17fBeb9'
});
console.log('Nonce:', nonce);
```
{% endcode %}
{% endtab %}
{% endtabs %}
