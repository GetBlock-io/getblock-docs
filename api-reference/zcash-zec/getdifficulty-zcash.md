---
description: >-
  Example code for the getdifficulty JSON-RPC method. Сomplete guide on how to
  use the getdifficulty JSON-RPC method in GetBlock.io Web3 documentation.
---

# getdifficulty - Zcash

This method returns the current proof-of-work difficulty as a floating-point multiple of the minimum difficulty. Zcash uses the Equihash proof-of-work algorithm; difficulty adjusts every block via a digishield-derived algorithm to target 75-second block times.

## Parameters

This method takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getdifficulty",
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
    method: 'getdifficulty',
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
        'method': 'getdifficulty',
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
            "method": "getdifficulty",
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
    "result": 229698549.25032642
}
```

## Response Parameters

| Parameter | Type   | Description                                                   |
| --------- | ------ | ------------------------------------------------------------- |
| `result`  | number | Difficulty as a multiplier over the minimum difficulty target |

## Use Cases

* **Mining Pool Statistics**: Display current difficulty on mining dashboards and calculators
* **Historical Analysis**: Track difficulty over time to visualize network hashrate trends
* **Reward Calculation**: Input to expected-reward calculations for miners choosing between chains

## Error Handling

| Error Code | Message        | Description                                                 |
| ---------- | -------------- | ----------------------------------------------------------- |
| -32603     | Internal error | Node has no blocks in state or failed to compute difficulty |
