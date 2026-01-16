---
description: >-
  Example code for the eth_blockNumber JSON-RPC method. Complete guide on how to
  use eth_blockNumber JSON-RPC in GetBlock Web3 documentation.
---

# eth\_blockNumber - Somnia

This method returns the number of the most recent block on the Somnia network. This is a fundamental method for tracking blockchain state, monitoring network progress, and determining transaction finality. Given Somnia's \~100ms block times, this value updates rapidly.

## Parameters

* None

## Returns

| Field  | Type   | Description                                      |
| ------ | ------ | ------------------------------------------------ |
| result | string | The current block number as a hexadecimal string |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_blockNumber",
  "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify(
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_blockNumber",
  "params": []
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

payload = json.dumps(
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_blockNumber",
  "params": []
})

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
        .body(r#"
          "jsonrpc": "2.0",
          "id": "getblock.io",
          "method": "eth_blockNumber",
          "params": []
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

{% code title="Response JSON" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x1a2b3c"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                             |
| --------- | ------ | ------------------------------------------------------- |
| result    | string | Hexadecimal block number (e.g., "0x1a2b3c" = 1,715,004) |

## Use Cases

* Monitor blockchain synchronization status
* Calculate transaction confirmations
* Track network liveness and progress
* Implement block-based polling for updates
* Determine finality for transactions

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32603     | Internal error - node processing issues |
| -32000     | Server error - RPC endpoint unavailable |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const blockNumber = await provider.getBlockNumber();
console.log(blockNumber);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const blockNumber = await client.getBlockNumber();
console.log(blockNumber);
```
{% endcode %}
{% endtab %}
{% endtabs %}
