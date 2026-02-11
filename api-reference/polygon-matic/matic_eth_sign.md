---
description: >-
  Example code for the eth_sign json-rpc method. Сomplete guide on how to use
  eth_sign json-rpc in GetBlock.io Web3 documentation.
---

# eth\_sign - Polygon

The `eth_sign` method calculates an Ethereum specific signature. Note that this method requires the node to have access to the account's private key, which is not available on shared infrastructure.

## Parameters

| Parameter | Type   | Required | Description                          |
| --------- | ------ | -------- | ------------------------------------ |
| address   | string | Yes      | 20-byte account address to sign with |
| message   | string | Yes      | Data to sign in hexadecimal          |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sign",
    "params": ["0xe7e2cb8c81c10ff191a73fe266788c9ce62ec754", "0xdeadbeaf"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_sign',
    params: ["0xe7e2cb8c81c10ff191a73fe266788c9ce62ec754", "0xdeadbeaf"],
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

{% tab title="Python (requests)" %}
{% code title="request.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_sign",
    "params": ["0xe7e2cb8c81c10ff191a73fe266788c9ce62ec754", "0xdeadbeaf"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust (reqwest)" %}
{% code title="main.rs" %}
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
            "method": "eth_sign",
            "params": ["0xe7e2cb8c81c10ff191a73fe266788c9ce62ec754", "0xdeadbeaf"],
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
    "error": {
        "code": -32000,
        "message": "unknown account"
    }
}
```

## Response Parameters

| Field   | Type   | Description                                    |
| ------- | ------ | ---------------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                         |
| id      | string | Request identifier                             |
| result  | varies | Signature on success, or error on shared nodes |

## Use Case

The `eth_sign` method is useful for:

* Local node usage
* Development testing

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32602      | Invalid params  | Invalid method parameters       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_sign', ["0xe7e2cb8c81c10ff191a73fe266788c9ce62ec754", "0xdeadbeaf"]);
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
    method: 'eth_sign',
    params: ["0xe7e2cb8c81c10ff191a73fe266788c9ce62ec754", "0xdeadbeaf"]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
