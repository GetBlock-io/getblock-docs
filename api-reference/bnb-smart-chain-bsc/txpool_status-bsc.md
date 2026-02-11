---
description: >-
  Example code for the txpool_status JSON RPC method. Сomplete guide on how to
  use txpool_status JSON RPC in GetBlock Web3 documentation.
---

# txpool\_status - BSC

This method returns the number of pending and queued transactions in the transaction pool on the BNB Smart Chain.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "txpool_status",
    "params": [],
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
    method: 'txpool_status',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const result = response.data.result;
    console.log('Pending:', parseInt(result.pending, 16));
    console.log('Queued:', parseInt(result.queued, 16));
})
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "txpool_status",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
result = response.json()["result"]
print(f"Pending: {int(result['pending'], 16)}")
print(f"Queued: {int(result['queued'], 16)}")
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
        "method": "txpool_status",
        "params": [],
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
    "result": {
        "pending": "0x10",
        "queued": "0x5"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                  |
| --------- | ------ | ---------------------------- |
| pending   | string | Pending tx count (0x10 = 16) |
| queued    | string | Queued tx count (0x5 = 5)    |

## Use Cases

* Monitor mempool congestion
* Estimate transaction confirmation times
* Track network activity
* Optimize gas prices

## Error Handling

| Error Code | Description          |
| ---------- | -------------------- |
| -32601     | Method not supported |
| -32603     | Internal error       |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const status = await provider.send('txpool_status', []);
console.log('Pending:', parseInt(status.pending, 16));
console.log('Queued:', parseInt(status.queued, 16));
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const status = await client.request({ method: 'txpool_status' });
console.log('Pending:', parseInt(status.pending, 16));
```
{% endtab %}
{% endtabs %}
