---
description: >-
  Example code for the eth_getStorageAt JSON RPC method. Сomplete guide on how
  to use eth_getStorageAt JSON RPC in GetBlock Web3 documentation.
---

# eth\_getStorageAt - BSC

This method returns the value from a storage position at a given address on the BNB Smart Chain. This allows reading raw contract storage, including private variables.

## Parameters

| Parameter   | Type   | Required | Description              |
| ----------- | ------ | -------- | ------------------------ |
| address     | string | Yes      | Contract address         |
| position    | string | Yes      | Storage position (hex)   |
| blockNumber | string | Yes      | Block number or "latest" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": [
        "0x55d398326f99059fF775485246999027B3197955",
        "0x0",
        "latest"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getStorageAt',
    params: [
        '0x55d398326f99059fF775485246999027B3197955',
        '0x0',
        'latest'
    ],
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

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": [
        "0x55d398326f99059fF775485246999027B3197955",
        "0x0",
        "latest"
    ],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
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
        "method": "eth_getStorageAt",
        "params": [
            "0x55d398326f99059fF775485246999027B3197955",
            "0x0",
            "latest"
        ],
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
    "result": "0x000000000000000000000000f68a4b64162906eff0ff6ae34e2bb1cd42fef62d"
}
```

## Response Parameters

| Parameter | Type   | Description           |
| --------- | ------ | --------------------- |
| result    | string | 32-byte storage value |

## Use Cases

* Read private contract variables
* Analyze proxy implementations
* Debug contract state
* Security research

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const storage = await provider.getStorage(
    '0x55d398326f99059fF775485246999027B3197955',
    0
);
console.log(storage);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const storage = await client.getStorageAt({
    address: '0x55d398326f99059fF775485246999027B3197955',
    slot: '0x0'
});
console.log(storage);
```
{% endcode %}
{% endtab %}
{% endtabs %}
