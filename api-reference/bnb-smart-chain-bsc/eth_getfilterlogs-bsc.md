---
description: >-
  Example code for the eth_getFilterLogs JSON RPC method. Сomplete guide on how
  to use eth_getFilterLogs JSON RPC in GetBlock Web3 documentation.
---

# eth\_getFilterLogs - BSC

The `eth_getFilterLogs` method returns an array of all logs matching a filter with the given ID on the BNB Smart Chain.

## Parameters

| Parameter | Type   | Required | Description                   |
| --------- | ------ | -------- | ----------------------------- |
| filterId  | string | Yes      | Filter ID from eth\_newFilter |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterLogs",
    "params": ["0x1"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getFilterLogs',
    params: ['0x1'],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getFilterLogs",
    "params": ["0x1"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_getFilterLogs",
        "params": ["0x1"],
        "id": "getblock.io"
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": []
}
```

## Response Parameters

| Parameter | Type  | Description                              |
| --------- | ----- | ---------------------------------------- |
| result    | array | Array of log objects matching the filter |

## Use Cases

* Retrieve all logs from a filter
* Batch process events
* Historical event analysis

## Error Handling

| Error Code | Description       |
| ---------- | ----------------- |
| -32602     | Invalid filter ID |
| -32603     | Internal error    |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Prefer using getLogs directly
const logs = await provider.getLogs({
    address: '0x55d398326f99059fF775485246999027B3197955',
    fromBlock: 'latest'
});
console.log(logs);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const logs = await client.getLogs({
    address: '0x55d398326f99059fF775485246999027B3197955'
});
console.log(logs);
```
{% endtab %}
{% endtabs %}
