---
description: >-
  Example code for the eth_getLogs JSON-RPC method. Complete guide on how to use
  eth_getLogs JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getlogs - Worldchain

Returns an array of all logs matching a given filter object on the World Chain network.

### Parameters

| Parameter    | Type   | Description                                                  |
| ------------ | ------ | ------------------------------------------------------------ |
| filterObject | object | Filter options including fromBlock, toBlock, address, topics |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{"fromBlock": "0x1A5C3B0", "toBlock": "0x1A5C3B2", "address": "0x1234567890123456789012345678901234567890", "topics": []}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{"fromBlock": "0x1A5C3B0", "toBlock": "0x1A5C3B2", "address": "0x1234567890123456789012345678901234567890", "topics": []}],
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
    "method": "eth_getLogs",
    "params": [{"fromBlock": "0x1A5C3B0", "toBlock": "0x1A5C3B2", "address": "0x1234567890123456789012345678901234567890", "topics": []}],
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
            "method": "eth_getLogs",
            "params": [{"fromBlock": "0x1A5C3B0", "toBlock": "0x1A5C3B2", "address": "0x1234567890123456789012345678901234567890", "topics": []}],
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
            "address": "0x1234567890123456789012345678901234567890",
            "topics": ["0x..."],
            "data": "0x..."
        }
    ]
}
```

### Response Parameters

| Field   | Type   | Description                             |
| ------- | ------ | --------------------------------------- |
| address | string | Contract address that generated the log |
| topics  | array  | Array of indexed log parameters         |
| data    | string | Non-indexed log data                    |

### Use Case

The `eth_getLogs` method on World Chain is typically used for:

* Event monitoring
* Contract event retrieval
* Historical event analysis
* Indexing

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

const logs = await provider.getLogs({
    fromBlock: 'latest',
    address: '0x1234567890123456789012345678901234567890'
});
console.log('Logs:', logs);
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

const logs = await client.getLogs({
    address: '0x1234567890123456789012345678901234567890'
});
console.log('Logs:', logs);
```
{% endtab %}
{% endtabs %}
