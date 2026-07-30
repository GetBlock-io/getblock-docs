---
description: >-
  Example code for the eth_maxPriorityFeePerGas json-rpc method. Сomplete guide
  on how to use eth_maxPriorityFeePerGas json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_maxPriorityFeePerGas - Polygon

The **eth\_maxPriorityFeePerGas** method returns a suggested priority fee (tip) for EIP-1559 transactions. This value represents the amount validators receive as an incentive.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_maxPriorityFeePerGas",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_maxPriorityFeePerGas',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python (requests)" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_maxPriorityFeePerGas",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust (reqwest)" %}
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
            "method": "eth_maxPriorityFeePerGas",
            "params": [],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

{% code title="Response (application/json)" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x59682f00"
}
```
{% endcode %}

## Response Parameters

| Field   | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                       |
| id      | string | Request identifier                           |
| result  | varies | Suggested priority fee in wei as hexadecimal |

## Use Case

The eth\_maxPriorityFeePerGas method is useful for:

* **EIP-1559 transactions**
* **Dynamic fee calculation**
* **Priority estimation**
* **Fast confirmation**

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32602      | Invalid params  | Invalid method parameters       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_maxPriorityFeePerGas', []);
console.log('Result:', result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_maxPriorityFeePerGas',
    params: []
});
console.log('Result:', result);
```
{% endtab %}
{% endtabs %}
