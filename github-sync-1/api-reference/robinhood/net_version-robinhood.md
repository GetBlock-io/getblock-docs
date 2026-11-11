---
description: >-
  Example code for the net_version JSON-RPC method. Complete guide to using the
  net_version JSON-RPC method in GetBlock.io Web3 documentation.
---

# net\_version - Robinhood

Returns the current network ID. For Robinhood Chain mainnet this is `"4663"` (returned as a decimal string, not hex like `eth_chainId`).

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "net_version",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "net_version",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "net_version",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "net_version",
        "params": [],
        "id": "getblock.io"
});

    let response = client
        .post("https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/")
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "4663"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                         |
| ------ | ------ | ------------------------------------------------------------------- |
| result | string | Network ID as decimal string — `"4663"` for Robinhood Chain mainnet |

## Use Cases

* Pre-flight network verification before submitting transactions
* Detecting mainnet vs testnet (testnet is `"46630"`)
* MetaMask network detection

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any method):
const result = await provider.send('net_version', []);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider,
// e.g. provider.getBalance(addr), provider.getBlock(n), provider.getTransaction(hash).
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/')
});

// Generic JSON-RPC request (works for any method):
const result = await client.request({
    method: 'net_version',
    params: []
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endtab %}
{% endtabs %}
