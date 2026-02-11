---
description: >-
  Example code for the eth_uninstallFilter JSON RPC method. Сomplete guide on
  how to use eth_uninstallFilter JSON RPC in GetBlock Web3 documentation.
---

# eth\_uninstallFilter - BSC

This method removes a filter with the given ID. Filters automatically time out when not polled for a period of time, but this method allows explicit removal.

## Parameters

| Parameter | Type   | Required | Description         |
| --------- | ------ | -------- | ------------------- |
| filterId  | string | Yes      | Filter ID to remove |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_uninstallFilter",
    "params": ["0x1"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_uninstallFilter',
    params: ['0x1'],
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

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_uninstallFilter",
    "params": ["0x1"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_uninstallFilter",
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": true
}
```
{% endcode %}

## Response Parameters

| Parameter | Type    | Description            |
| --------- | ------- | ---------------------- |
| result    | boolean | Removal success status |

## Use Cases

* Clean up unused filters
* Resource management
* Proper filter lifecycle handling

## Error Handling

| Error Code | Description       |
| ---------- | ----------------- |
| -32602     | Invalid filter ID |
| -32603     | Internal error    |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// SDKs handle filter cleanup automatically
// Manual removal via raw RPC if needed
const removed = await provider.send('eth_uninstallFilter', ['0x1']);
console.log('Filter removed:', removed);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// Use unwatch function returned by watch methods
const unwatch = client.watchBlocks({
    onBlock: block => console.log(block)
});

// Later: cleanup
unwatch();
```
{% endcode %}
{% endtab %}
{% endtabs %}
