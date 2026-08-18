---
description: >-
  Example code for the getnetworksolps JSON-RPC method. Сomplete guide on how to
  use the getnetworksolps JSON-RPC method in GetBlock.io Web3 documentation.
---

# getnetworksolps - Zcash

This method returns an estimate of the network's Equihash solutions per second over a window of recent blocks. This is Zcash's equivalent of Bitcoin's `getnetworkhashps` — Zcash's Equihash algorithm is measured in solutions/second (sol/s) rather than hashes/second.

## Parameters

| Parameter | Type    | Required | Description                                                                                                                  |
| --------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `blocks`  | integer | No       | Number of blocks to average over. Default `120`. Use `-1` for the full averaging window since the last difficulty adjustment |
| `height`  | integer | No       | Block height at which to estimate. Default `-1` (current tip)                                                                |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getnetworksolps",
    "params": [
        120,
        -1
    ],
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
    method: 'getnetworksolps',
    params: [
        120,
        -1
    ],
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
        'method': 'getnetworksolps',
        'params': [
        120,
        -1
    ],
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
            "method": "getnetworksolps",
            "params": [
        120,
        -1
    ],
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
    "result": 20929098466
}
```

## Response Parameters

| Parameter | Type    | Description                                     |
| --------- | ------- | ----------------------------------------------- |
| `result`  | integer | Estimated network Equihash solutions per second |

## Use Cases

* **Network Hashrate Charts**: Feed historical `networksolps` values into visualization dashboards
* **Mining Profitability Calculators**: Combined with block reward and difficulty to project expected earnings
* **Congestion vs Capacity Analysis**: Compare hashrate trends to network activity to understand security margins

## Error Handling

| Error Code | Message        | Description                             |
| ---------- | -------------- | --------------------------------------- |
| -32602     | Invalid params | Blocks or height parameter is malformed |
| -32603     | Internal error | Node failed to compute the estimate     |
