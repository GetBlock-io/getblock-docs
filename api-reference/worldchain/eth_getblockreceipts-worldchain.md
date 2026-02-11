---
description: >-
  Example code for the eth_getBlockReceipts JSON-RPC method. Complete guide on
  how to use eth_getBlockReceipts JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getblockreceipts - Worldchain

Returns all transaction receipts for a given block on the World Chain network.

### Parameters

| Parameter   | Type   | Description                                             |
| ----------- | ------ | ------------------------------------------------------- |
| blockNumber | string | Block number in hex, or "latest", "earliest", "pending" |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": ["0x1A5C3B2"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": ["0x1A5C3B2"],
    "id": "getblock.io"
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
{% endtab %}

{% tab title="Python (requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": ["0x1A5C3B2"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
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
            "method": "eth_getBlockReceipts",
            "params": ["0x1A5C3B2"],
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

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "transactionHash": "0x...",
            "status": "0x1",
            "gasUsed": "0x..."
        }
    ]
}
```

### Response Parameters

| Field  | Type  | Description                                        |
| ------ | ----- | -------------------------------------------------- |
| result | array | Array of transaction receipt objects for the block |

### Use Case

The `eth_getBlockReceipts` method on World Chain is typically used for:

* Bulk receipt retrieval
* Block analysis
* Gas usage analysis
* Transaction verification

### Error Handling

| Status Code | Error Message | Cause                           |
| ----------- | ------------- | ------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const receipts = await provider.send('eth_getBlockReceipts', ['latest']);
console.log('Receipts:', receipts);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { worldchain } from 'viem/chains';

const client = createPublicClient({
    chain: worldchain,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const receipts = await client.request({
    method: 'eth_getBlockReceipts',
    params: ['latest']
});
console.log('Receipts:', receipts);
```
{% endtab %}
{% endtabs %}
