---
description: >-
  Example code for the getdifficulty JSON-RPC method. Complete guide on how to
  use the getdifficulty JSON-RPC method in the GetBlock Web3 documentation.
---

# getdifficulty - Bitcoin Cash

This method returns the proof-of-work difficulty as a multiple of the minimum difficulty. Higher values indicate more hashing work is required per block.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'getdifficulty', params: [], id: 'getblock.io'
});
console.log(data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getdifficulty',
        'params': [],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
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
    "error": null,
    "id": "getblock.io",
    "result": 303698825886.4642
}
```

## Response Parameters

| Parameter | Type         | Description                                                          |
| --------- | ------------ | -------------------------------------------------------------------- |
| error     | null\|object | Error object when the call fails, otherwise null                     |
| id        | string       | Request identifier matching the request                              |
| result    | numeric      | Current proof-of-work difficulty as a multiple of minimum difficulty |

## Use Cases

* **Mining Estimates**: Estimate expected time to find a block at a given hash rate
* **Hashrate Derivation**: Combine with block interval to approximate network hashrate
* **Difficulty Charts**: Track difficulty adjustments over time
* **Profitability Models**: Feed difficulty into mining revenue calculations

## Error Handling

| Error Code | Message         | Description                                   |
| ---------- | --------------- | --------------------------------------------- |
| -32700     | Parse error     | Request body is not valid JSON                |
| -32600     | Invalid request | The JSON sent is not a valid request object   |
| -32603     | Internal error  | Node failed to read the requested chain state |
