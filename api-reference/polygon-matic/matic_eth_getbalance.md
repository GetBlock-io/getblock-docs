---
description: >-
  Example code for the eth_getBalance json-rpc method. Сomplete guide on how to
  use eth_getBalance json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBalance - Polygon

The **eth\_getBalance** method returns the balance of the account at the given address. The balance is returned in wei, which is the smallest unit of MATIC (1 MATIC = 10^18 wei).

## Parameters

| Parameter      | Type   | Required | Description                                                                  |
| -------------- | ------ | -------- | ---------------------------------------------------------------------------- |
| address        | string | Yes      | The 20-byte account address to check balance                                 |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending", "safe", "finalized" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1", "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getBalance',
    params: ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1", "latest"],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python (requests)" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1", "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust (reqwest)" %}
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
            "method": "eth_getBalance",
            "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1", "latest"],
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
    "result": "0x56bc75e2d63100000"
}
```

## Response Parameters

| Field   | Type   | Description                                |
| ------- | ------ | ------------------------------------------ |
| jsonrpc | string | JSON-RPC version (2.0)                     |
| id      | string | Request identifier                         |
| result  | varies | The balance in wei as a hexadecimal string |

## Use Case

The eth\_getBalance method is useful for:

* **Check wallet balances**
* **Verify sufficient funds before transactions**
* **Track account value changes**
* **Build portfolio trackers**

## Error Handling

| Status Code / Error | Cause                                       |
| ------------------- | ------------------------------------------- |
| 403                 | Forbidden — Missing or invalid ACCESS-TOKEN |
| -32600              | Invalid Request — Malformed request body    |
| -32602              | Invalid params — Invalid method parameters  |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_getBalance', ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1", "latest"]);
console.log('Result:', result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_getBalance',
    params: ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1", "latest"]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
