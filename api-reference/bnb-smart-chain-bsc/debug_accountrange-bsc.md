---
description: >-
  Example code for the debug_accountRange JSON RPC method. Сomplete guide on how
  to use debug_accountRange JSON RPC in GetBlock Web3 documentation.
---

# debug\_accountRange - BSC

The `debug_accountRange` method enumerates all accounts at a given block with paging support on the BNB Smart Chain.

## Parameters

| Parameter         | Type   | Required | Description           |
| ----------------- | ------ | -------- | --------------------- |
| blockHashOrNumber | string | Yes      | Block hash or number  |
| txIndex           | number | Yes      | Transaction index     |
| addressHash       | string | Yes      | Starting address hash |
| maxResults        | number | Yes      | Maximum results       |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_accountRange",
    "params": ["0x2625a00", 0, "0x0000000000000000000000000000000000000000000000000000000000000000", 10],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'debug_accountRange',
    params: ['0x2625a00', 0, '0x0000000000000000000000000000000000000000000000000000000000000000', 10],
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
    "method": "debug_accountRange",
    "params": ["0x2625a00", 0, "0x0000000000000000000000000000000000000000000000000000000000000000", 10],
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
        "method": "debug_accountRange",
        "params": ["0x2625a00", 0, "0x00000000...", 10],
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
        "accounts": {
            "0x...": {"balance": "0x...", "nonce": 1}
        },
        "next": "0x..."
    }
}
```

## Response Parameters

| Parameter | Type   | Description              |
| --------- | ------ | ------------------------ |
| accounts  | object | Account data map         |
| next      | string | Next hash for pagination |

## Use Cases

* Enumerate accounts at block
* State analysis
* Account enumeration

## Error Handling

| Error Code | Description          |
| ---------- | -------------------- |
| -32602     | Invalid params       |
| -32601     | Method not supported |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const accounts = await provider.send('debug_accountRange', ['0x2625a00', 0, '0x00...', 10]);
console.log(accounts);
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

const accounts = await client.request({
    method: 'debug_accountRange',
    params: ['0x2625a00', 0, '0x00...', 10]
});
```
{% endtab %}
{% endtabs %}
