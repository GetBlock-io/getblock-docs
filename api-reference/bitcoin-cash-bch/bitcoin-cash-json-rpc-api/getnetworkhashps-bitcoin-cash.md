---
description: >-
  Example code for the getnetworkhashps JSON-RPC method. Complete guide on how
  to use the getnetworkhashps JSON-RPC method in the GetBlock Web3
  documentation.
---

# getnetworkhashps - Bitcoin Cash

This method estimates the network hashes per second based on the last N blocks. A negative block count estimates since the last difficulty change.

## Parameters

| Parameter | Type    | Required | Description                                                                           |
| --------- | ------- | -------- | ------------------------------------------------------------------------------------- |
| nblocks   | numeric | No       | Number of blocks to average over, or -1 since the last difficulty change. Default 120 |
| height    | numeric | No       | Estimate at the given height instead of the tip. Default -1                           |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getnetworkhashps",
    "params": [120, -1],
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
  jsonrpc: '2.0', method: 'getnetworkhashps',
  params: [120, -1], id: 'getblock.io'
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
        'method': 'getnetworkhashps',
        'params': [120, -1],
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
            "method": "getnetworkhashps",
            "params": [120, -1],
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
    "result": 2.333876557488516e+18
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | numeric      | Estimated network hash rate in hashes per second |

## Use Cases

* **Security Metrics**: Report the hashing power securing the chain
* **Window Comparison**: Compare short and long averaging windows for volatility
* **Historical Analysis**: Estimate hash rate at a past height
* **Difficulty Modelling**: Predict the next difficulty adjustment

## Error Handling

| Error Code | Message         | Description                                   |
| ---------- | --------------- | --------------------------------------------- |
| -32700     | Parse error     | Request body is not valid JSON                |
| -32600     | Invalid request | The JSON sent is not a valid request object   |
| -32603     | Internal error  | Node failed to read the requested chain state |
