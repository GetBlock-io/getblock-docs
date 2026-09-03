---
description: >-
  Example code for the starknet_getStorageAt JSON-RPC method. Complete guide on
  how to use starknet_getStorageAt JSON-RPC in GetBlock Web3 documentation.
---

# starknet\_getStorageAt - STRK

This method returns the felt value stored at a given storage key of a contract at a specific block. It is used to read raw contract state directly.

## Parameters

| Parameter         | Type             | Required | Description                                             |
| ----------------- | ---------------- | -------- | ------------------------------------------------------- |
| contract\_address | string           | Yes      | The contract to read from                               |
| key               | string           | Yes      | The storage key (felt), typically a storage-var address |
| block\_id         | object \| string | Yes      | {block\_number} or {block\_hash}, or a tag              |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getStorageAt",
    "params": ["0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "0x0206f38f7e4f15e87567361213c28f235cccdaa1d7fd34c9db1dfe9489c6a091", "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'starknet_getStorageAt',
    params: ['0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', '0x0206f38f7e4f15e87567361213c28f235cccdaa1d7fd34c9db1dfe9489c6a091', 'latest'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'starknet_getStorageAt',
        'params': ['0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', '0x0206f38f7e4f15e87567361213c28f235cccdaa1d7fd34c9db1dfe9489c6a091', 'latest'],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "starknet_getStorageAt",
            "params": ["0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "0x0206f38f7e4f15e87567361213c28f235cccdaa1d7fd34c9db1dfe9489c6a091", "latest"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
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
    "result": "0x2386f26fc10000"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Felt value stored at the requested key  |

## Use Cases

* **Raw State Reads**: Read a storage slot that has no view function
* **Proxy Inspection**: Read an implementation pointer from a known slot
* **Mapping Access**: Read a mapping entry by its computed storage address
* **Audit & Forensics**: Verify on-chain state during analysis

## Error Handling

| Error                     | Message            | Description                                     |
| ------------------------- | ------------------ | ----------------------------------------------- |
| 20 / CONTRACT\_NOT\_FOUND | Contract not found | No contract exists at the address at that block |
| 24 / BLOCK\_NOT\_FOUND    | Block not found    | No block matches the supplied block\_id         |
