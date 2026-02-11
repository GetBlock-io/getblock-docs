---
description: >-
  Example code for the debug_storageRangeAt JSON RPC method. Сomplete guide on
  how to use debug_storageRangeAt JSON RPC in GetBlock Web3 documentation.
---

# debug\_storageRangeAt - BSC

The `debug_storageRangeAt` method returns the storage range for a contract at a specific block on the BNB Smart Chain.

## Parameters

| Parameter  | Type   | Required | Description          |
| ---------- | ------ | -------- | -------------------- |
| blockHash  | string | Yes      | Block hash           |
| txIndex    | number | Yes      | Transaction index    |
| address    | string | Yes      | Contract address     |
| startKey   | string | Yes      | Starting storage key |
| maxResults | number | Yes      | Maximum results      |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_storageRangeAt",
    "params": [
        "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
        0,
        "0x55d398326f99059fF775485246999027B3197955",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        10
    ],
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
    method: 'debug_storageRangeAt',
    params: [
        '0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd',
        0,
        '0x55d398326f99059fF775485246999027B3197955',
        '0x0000000000000000000000000000000000000000000000000000000000000000',
        10
    ],
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
    "method": "debug_storageRangeAt",
    "params": [
        "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
        0,
        "0x55d398326f99059fF775485246999027B3197955",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        10
    ],
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
        "method": "debug_storageRangeAt",
        "params": ["0x4e3a...", 0, "0x55d3...", "0x00...", 10],
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
        "storage": {
            "0x...": {"key": "0x...", "value": "0x..."}
        },
        "nextKey": "0x..."
    }
}
```

## Response Parameters

| Parameter | Type   | Description             |
| --------- | ------ | ----------------------- |
| storage   | object | Storage key-value pairs |
| nextKey   | string | Next key for pagination |

## Use Cases

* Analyze contract storage
* Debug storage issues
* State inspection

## Error Handling

| Error Code | Description     |
| ---------- | --------------- |
| -32602     | Invalid params  |
| -32000     | Block not found |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const storage = await provider.send('debug_storageRangeAt', [
    '0x4e3a...',
    0,
    '0x55d3...',
    '0x00...',
    10
]);
console.log(storage);
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

const storage = await client.request({
    method: 'debug_storageRangeAt',
    params: ['0x4e3a...', 0, '0x55d3...', '0x00...', 10]
});
```
{% endtab %}
{% endtabs %}
