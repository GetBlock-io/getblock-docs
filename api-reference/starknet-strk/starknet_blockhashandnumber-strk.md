---
description: >-
  Example code for the starknet_blockHashAndNumber JSON-RPC method. Complete
  guide on how to use starknet_blockHashAndNumber JSON-RPC in GetBlock Web3
  documentation.
---

# starknet\_blockHashAndNumber - STRK

This method returns both the hash and the number of the most recent accepted block in a single call. It is used to pin reads to a known tip.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_blockHashAndNumber",
    "params": [],
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
    method: 'starknet_blockHashAndNumber',
    params: [],
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
        'method': 'starknet_blockHashAndNumber',
        'params': [],
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
            "method": "starknet_blockHashAndNumber",
            "params": [],
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
    "result": {
        "block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4",
        "block_number": 700000
    }
}
```

## Response Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")         |
| id        | string | Request identifier matching the request   |
| result    | object | Object with block\_hash and block\_number |

## Use Cases

* **Consistent Reads**: Pin a batch of queries to one block hash and number
* **Reorg Detection**: Compare successive hashes at the same height
* **Explorer Backends**: Show the latest block hash and number together
* **Snapshotting**: Record the tip before a multi-call read

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| 63 / UNEXPECTED\_ERROR    | Unexpected error | The node failed to return the latest block        |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
