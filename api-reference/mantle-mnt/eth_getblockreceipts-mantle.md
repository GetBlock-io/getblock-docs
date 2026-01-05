---
description: >-
  Example code for the eth_getBlockReceipts JSON-RPC method. Complete guide on
  how to use eth_getBlockReceipts JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockReceipts - Mantle

This method returns all transaction receipts for a given block. It provides a more efficient way to retrieve all receipts in a block compared to fetching them individually.

### Parameters

| Parameter       | Type   | Description                                                 |
| --------------- | ------ | ----------------------------------------------------------- |
| blockIdentifier | string | Block number (hex) or tag ("latest", "earliest", "pending") |

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": ["0x3F6777"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="request.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": ["0x3F6777"],
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
{% endcode %}
{% endtab %}

{% tab title="Python (requests)" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": ["0x3F6777"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
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
            "method": "eth_getBlockReceipts",
            "params": ["0x3F6777"],
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

### Response

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "blockHash": "0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e",
            "blockNumber": "0x3f6777",
            "contractAddress": null,
            "cumulativeGasUsed": "0x5208",
            "from": "0x52988d3dd2d1b36e9c48866670b7683c42197139",
            "gasUsed": "0x5208",
            "logs": [],
            "logsBloom": "0x00000...",
            "status": "0x1",
            "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "transactionHash": "0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8",
            "transactionIndex": "0x0",
            "type": "0x2"
        }
    ]
}
```
{% endcode %}

### Response Parameters

| Field             | Type   | Description                                    |
| ----------------- | ------ | ---------------------------------------------- |
| result            | array  | Array of transaction receipt objects           |
| blockHash         | string | Hash of the block containing the transaction   |
| blockNumber       | string | Block number (hex)                             |
| contractAddress   | string | Contract address if deployment, null otherwise |
| cumulativeGasUsed | string | Total gas used in block up to this tx          |
| gasUsed           | string | Gas used by this transaction                   |
| logs              | array  | Array of log objects emitted                   |
| status            | string | 0x1 for success, 0x0 for failure               |

### Use Case

The `eth_getBlockReceipts` method is essential for:

* Block explorers retrieving all receipts efficiently
* Data indexing services
* Batch transaction analysis
* Event log collection for entire blocks
* Transaction verification systems

### Error Handling

| Status Code | Error Message  | Cause                           |
| ----------- | -------------- | ------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params | Invalid block identifier        |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const receipts = await provider.send('eth_getBlockReceipts', ['0x3F6777']);
console.log('Block Receipts:', receipts);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const receipts = await client.request({
    method: 'eth_getBlockReceipts',
    params: ['0x3F6777']
});
console.log('Block Receipts:', receipts);
```
{% endtab %}
{% endtabs %}
