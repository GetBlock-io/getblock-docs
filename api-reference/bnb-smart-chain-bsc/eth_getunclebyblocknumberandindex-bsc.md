---
description: >-
  Example code for the eth_getUncleByBlockNumberAndIndex JSON RPC method.
  Сomplete guide on how to use eth_getUncleByBlockNumberAndIndex JSON RPC in
  GetBlock Web3 documentation.
---

# eth\_getUncleByBlockNumberAndIndex - BSC

This method gets uncle block information by block number and uncle index on BSC. BSC uses PoSA consensus and typically does not produce uncle blocks.

## Parameters

| Parameter   | Type   | Required | Description                    |
| ----------- | ------ | -------- | ------------------------------ |
| blockNumber | string | Yes      | Block number (hex) or "latest" |
| index       | string | Yes      | Uncle index (hex)              |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getUncleByBlockNumberAndIndex",
    "params": ["latest", "0x0"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getUncleByBlockNumberAndIndex',
    params: ['latest', '0x0'],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getUncleByBlockNumberAndIndex",
    "params": ["latest", "0x0"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_getUncleByBlockNumberAndIndex",
        "params": ["latest", "0x0"],
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
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": null
}
```

## Response Parameters

| Parameter | Type        | Description                         |
| --------- | ----------- | ----------------------------------- |
| result    | object/null | Uncle block (typically null on BSC) |

## Use Cases

* EVM compatibility checks
* Cross-chain development

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const uncle = await provider.send('eth_getUncleByBlockNumberAndIndex', ['latest', '0x0']);
console.log('Uncle:', uncle);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// BSC does not produce uncle blocks
```
{% endtab %}
{% endtabs %}
