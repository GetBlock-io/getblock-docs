# eth\_uninstallFilter - Cronos

This method removes a filter by ID and stops the node from tracking it. Filters also expire automatically after a period of inactivity.

## Parameters

| Parameter | Type   | Required | Description                                  |
| --------- | ------ | -------- | -------------------------------------------- |
| filterId  | string | Yes      | Filter ID returned by a create-filter method |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_uninstallFilter",
    "params": ["0x1a4b0d21f8e0c3b6d2a9f7e4c1b8a5d2"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_uninstallFilter',
    params: ['0x1a4b0d21f8e0c3b6d2a9f7e4c1b8a5d2'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_uninstallFilter',
        'params': ['0x1a4b0d21f8e0c3b6d2a9f7e4c1b8a5d2'],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_uninstallFilter",
            "params": ["0x1a4b0d21f8e0c3b6d2a9f7e4c1b8a5d2"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
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
    "result": true
}
```

## Response Parameters

| Parameter | Type    | Description                                |
| --------- | ------- | ------------------------------------------ |
| jsonrpc   | string  | JSON-RPC protocol version ("2.0")          |
| id        | string  | Request identifier matching the request    |
| result    | boolean | true if the filter existed and was removed |

## Use Cases

* **Resource Cleanup**: Release a filter once polling is complete
* **Filter Rotation**: Remove stale filters before creating new ones
* **Leak Prevention**: Avoid accumulating unused filters on the node
* **Graceful Shutdown**: Tear down filters when a watcher stops

## Error Handling

| Error Code | Message        | Description                          |
| ---------- | -------------- | ------------------------------------ |
| -32602     | Invalid params | Malformed or unknown filter ID       |
| -32603     | Internal error | The node failed to remove the filter |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// ethers v6 manages listeners; use provider.removeAllListeners()
provider.removeAllListeners();
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

// viem returns an unwatch function from watch* helpers
unwatch();
```
{% endcode %}
{% endtab %}
{% endtabs %}
