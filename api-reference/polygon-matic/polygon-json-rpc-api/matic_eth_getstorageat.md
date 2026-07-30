---
description: >-
  Example code for the eth_getStorageAt json-rpc method. Сomplete guide on how
  to use eth_getStorageAt json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getStorageAt - Polygon

The **eth\_getStorageAt** method returns the value from a storage position at a given address. This allows direct access to contract storage slots.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| address        | string | Yes      | 20-byte contract address                                |
| position       | string | Yes      | Storage position as hexadecimal                         |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", "0x0", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getStorageAt',
    params: ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", "0x0", "latest"],
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
{% code title="example.py" overflow="wrap" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", "0x0", "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
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
            "method": "eth_getStorageAt",
            "params": ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", "0x0", "latest"],
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
    "result": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```

## Response Parameters

| Field   | Type   | Description                           |
| ------- | ------ | ------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                |
| id      | string | Request identifier                    |
| result  | varies | 32-byte value at the storage position |

## Use Case

The eth\_getStorageAt method is useful for:

* **Contract debugging**
* **State analysis**
* **Security auditing**
* **Reading internal variables**

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32602      | Invalid params  | Invalid method parameters       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_getStorageAt', ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", "0x0", "latest"]);
console.log('Result:', result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_getStorageAt',
    params: ["0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0", "0x0", "latest"]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
