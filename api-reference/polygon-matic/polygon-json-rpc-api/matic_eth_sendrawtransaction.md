---
description: >-
  Example code for the eth_sendRawTransaction json-rpc method. Сomplete guide on
  how to use eth_sendRawTransaction json-rpc in GetBlock.io Web3 documentation.
---

# eth\_sendRawTransaction - Polygon

The **eth\_sendRawTransaction** method submits a pre-signed transaction for broadcast to the network. The transaction must be signed with the sender's private key before calling this method.

## Parameters

| Parameter             | Type   | Required | Description                            |
| --------------------- | ------ | -------- | -------------------------------------- |
| signedTransactionData | string | Yes      | Signed transaction data in hexadecimal |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": ["0xf86c0885048c273950..."],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="sendRawTransaction.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_sendRawTransaction',
    params: ["0xf86c0885048c273950..."],
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
{% code title="send_raw_transaction.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": ["0xf86c0885048c273950..."],
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
            "method": "eth_sendRawTransaction",
            "params": ["0xf86c0885048c273950..."],
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

{% code title="Response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
}
```
{% endcode %}

## Response Parameters

| Field   | Type   | Description                                              |
| ------- | ------ | -------------------------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                                   |
| id      | string | Request identifier                                       |
| result  | varies | 32-byte transaction hash, or error if transaction failed |

## Use Case

The eth\_sendRawTransaction method is useful for:

* **Transaction submission**
* **Token transfers**
* **Contract interaction**
* **DeFi operations**

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32602      | Invalid params  | Invalid method parameters       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-send.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_sendRawTransaction', ["0xf86c0885048c273950..."]);
console.log('Result:', result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-send.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_sendRawTransaction',
    params: ["0xf86c0885048c273950..."]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
