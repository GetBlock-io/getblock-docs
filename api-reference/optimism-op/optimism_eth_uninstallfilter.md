---
description: >-
  Example code for the eth_uninstallFilter json-rpc method. Сomplete guide on
  how to use eth_uninstallFilter json-rpc in GetBlock.io Web3 documentation.
---

# eth\_uninstallFilter - Optimism

This method removes a filter that was created with `eth_newFilter`, `eth_newBlockFilter`, or `eth_newPendingTransactionFilter`. Filters automatically time out if not polled, but it's good practice to uninstall filters when they're no longer needed.

### Parameters

| Parameter | Type     | Description                 | Required |
| --------- | -------- | --------------------------- | -------- |
| filterId  | QUANTITY | The filter ID to uninstall. | Yes      |

### Request Sample

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

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": false
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: Boolean indicating whether the filter was successfully uninstalled.

### Use Case

The eth\_uninstallFilter method is commonly used for:

* **Resource cleanup**
* **Filter management**
* **Connection cleanup**

## Error Handling

| Error Code | Description       |
| ---------- | ----------------- |
| -32602     | Invalid filter ID |
| -32603     | Internal error    |

## Web3 Integration

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
import { optimism } from 'viem/chains';

const client = createPublicClient({
    chain: optimism,
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
