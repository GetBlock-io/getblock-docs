---
description: >-
  Example code for the eth_sendRawTransaction JSON-RPC method. Complete guide on
  how to use eth_sendRawTransaction JSON-RPC in GetBlock Web3 documentation
---

# eth\_sendRawTransaction - SEI

Submits a pre-signed transaction to the Sei network for broadcast. Returns the transaction hash if successful.

## Parameters

| Parameter         | Type   | Description                                 |
| ----------------- | ------ | ------------------------------------------- |
| signedTransaction | string | The signed transaction data as a hex string |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": ["0xf86c808504a817c80082520894..."],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="sendRawTransaction.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_sendRawTransaction',
    params: ["0xf86c808504a817c80082520894..."],
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
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": ["0xf86c808504a817c80082520894..."],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust (reqwest)" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_sendRawTransaction",
            "params": ["0xf86c808504a817c80082520894..."],
            "id": "getblock.io"
        }))
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

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
}
```

## Response Parameters

| Field  | Type   | Description                                       |
| ------ | ------ | ------------------------------------------------- |
| result | string | The transaction hash of the submitted transaction |

## Use Case

The `eth_sendRawTransaction` method is essential for:

* Blockchain developers building applications on Sei
* Wallet applications requiring network data
* Analytics platforms tracking Sei network activity
* DeFi protocols integrating with Sei's parallelized EVM

## Error Handling

Common errors when using this method:

| Error Code | Message          | Description                                |
| ---------- | ---------------- | ------------------------------------------ |
| -32700     | Parse error      | Invalid JSON                               |
| -32600     | Invalid Request  | JSON is not a valid request object         |
| -32601     | Method not found | Method does not exist                      |
| -32602     | Invalid params   | Invalid method parameters                  |
| -32603     | Internal error   | Internal JSON-RPC error                    |
| -32000     | Invalid input    | Generic input error                        |
| -32500     | Cross-VM error   | Error in cross-VM operation (Sei-specific) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Using ethers provider methods
const result = await provider.send('eth_sendRawTransaction', ["0xf86c808504a817c80082520894..."]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { sei } from 'viem/chains';

const client = createPublicClient({
    chain: sei,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// Using viem's request method
const result = await client.request({
    method: 'eth_sendRawTransaction',
    params: ["0xf86c808504a817c80082520894..."]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
