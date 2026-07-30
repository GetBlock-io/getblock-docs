---
description: >-
  Example code for the eth_getLogs json-rpc method. Сomplete guide on how to use
  eth_getLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getLogs - Polygon

The **eth\_getLogs** method returns an array of all logs matching the given filter object. This is the most flexible way to query event logs without creating a persistent filter.

## Parameters

| Parameter    | Type   | Required | Description                                                      |
| ------------ | ------ | -------- | ---------------------------------------------------------------- |
| filterObject | object | Yes      | Filter options including fromBlock, toBlock, address, and topics |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{"fromBlock": "0x3A2B5C0", "toBlock": "0x3A2B5C7", "address": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0"}],
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
    method: 'eth_getLogs',
    params: [{"fromBlock": "0x3A2B5C0", "toBlock": "0x3A2B5C7", "address": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0"}],
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
    "method": "eth_getLogs",
    "params": [{"fromBlock": "0x3A2B5C0", "toBlock": "0x3A2B5C7", "address": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0"}],
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
            "method": "eth_getLogs",
            "params": [{"fromBlock": "0x3A2B5C0", "toBlock": "0x3A2B5C7", "address": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0"}],
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
    "result": [{
        "address": "0x...",
        "topics": ["0x..."],
        "data": "0x...",
        "blockNumber": "0x3A2B5C7",
        "transactionHash": "0x..."
    }]
}
```

## Response Parameters

| Field   | Type   | Description                                       |
| ------- | ------ | ------------------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                            |
| id      | string | Request identifier                                |
| result  | varies | Array of log objects matching the filter criteria |

## Use Case

The eth\_getLogs method is useful for:

* **Event indexing**
* **Transfer tracking**
* **DEX analysis**
* **NFT monitoring**

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

const result = await provider.send('eth_getLogs', [{"fromBlock": "0x3A2B5C0", "toBlock": "0x3A2B5C7", "address": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0"}]);
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
    method: 'eth_getLogs',
    params: [{"fromBlock": "0x3A2B5C0", "toBlock": "0x3A2B5C7", "address": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0"}]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
