---
description: >-
  Example code for the eth_getStorageAt json-rpc method. Сomplete guide on how
  to use eth_getStorageAt json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getStorageAt - Optimism

This method reads directly from a contract's storage slots. It's a low-level method that requires knowledge of the contract's storage layout. Each storage slot holds 32 bytes of data. This is useful for reading contract state that isn't exposed through public functions or for debugging and analysis purposes.

### Parameters

| Parameter | Type           | Description                                              | Required |
| --------- | -------------- | -------------------------------------------------------- | -------- |
| address   | DATA, 20 Bytes | The address of the contract.                             | Yes      |
| position  | QUANTITY       | The storage position (slot) as a hexadecimal.            | Yes      |
| block     | QUANTITY\|TAG  | Block number in hex, or 'latest', 'earliest', 'pending'. | Yes      |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
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
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
    "id": "getblock.io"
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
{% code title="request.py" overflow="wrap" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
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
    "params": ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
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

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x577261707065642045746865720000000000000000000000000000000000001a"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The 32-byte value at the storage position as a hexadecimal string.

### Use Case

The eth\_getStorageAt method is commonly used for:

* **Contract state analysis**
* **Debugging**
* **Storage layout exploration**
* **Security auditing**

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |
| -32603     | Internal error |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const storage = await provider.getStorage(
    "0x4200000000000000000000000000000000000006", "0x0", "latest"
);
console.log(storage);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

const client = createPublicClient({
    chain: optimism,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const storage = await client.getStorageAt({
    address: '0x4200000000000000000000000000000000000006,
    slot: '0x0',
    latest
});
console.log(storage);
```
{% endcode %}
{% endtab %}
{% endtabs %}
