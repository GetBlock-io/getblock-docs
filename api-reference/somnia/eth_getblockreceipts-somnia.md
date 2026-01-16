---
description: >-
  Example code for the eth_getBlockReceipts JSON-RPC method. Complete guide on
  how to use eth_getBlockReceipts JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockReceipts - Somnia

This method returns all transaction receipts for a given block on the Somnia network. This is efficient for indexing entire blocks of transaction data.

## Parameters

| Parameter   | Type   | Required | Description                    |
| ----------- | ------ | -------- | ------------------------------ |
| blockNumber | string | Yes      | Block number (hex) or "latest" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getBlockReceipts",
  "params": ["latest"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getBlockReceipts",
  "params": ["latest"]
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getBlockReceipts",
  "params": ["latest"]
}})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
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
          "id": "getblock.io",
          "method": "eth_getBlockReceipts",
          "params": ["latest"]
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

## Response Example

{% code title="Response (JSON)" overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": [
    {
      "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
      "status": "0x1",
      "gasUsed": "0x5208",
      "logs": []
    }
  ]
}
```
{% endcode %}

## Use Cases

* Block indexing services
* Batch receipt retrieval
* Analytics and monitoring
* Building block explorers

##

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');
const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');
const receipts = await provider.send('eth_getBlockReceipts', ['latest']);
console.log(receipts);
```
{% endcode %}
{% endtab %}
{% endtabs %}
