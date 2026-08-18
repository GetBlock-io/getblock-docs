---
description: >-
  Example code for the getmininginfo JSON-RPC method. Сomplete guide on how to
  use the getmininginfo JSON-RPC method in GetBlock.io Web3 documentation.
---

# getmininginfo - Zcash

This method returns current mining-related state: block height, difficulty, transactions in the current block being built, and network hashes-per-second estimate. Useful for mining dashboards and pool operators tracking network conditions.

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
    "method": "getmininginfo",
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
    method: 'getmininginfo',
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
        'method': 'getmininginfo',
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
            "method": "getmininginfo",
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
        "blocks": 3420503,
        "currentblocksize": 2051,
        "currentblocktx": 2,
        "networksolps": 20929098466,
        "networkhashps": 20929098466,
        "chain": "main",
        "testnet": false
    }
}
```

## Response Parameters

| Parameter          | Type    | Description                                                   |
| ------------------ | ------- | ------------------------------------------------------------- |
| `blocks`           | integer | Current chain tip height                                      |
| `currentblocksize` | integer | Size of the current block being built by the miner (bytes)    |
| `currentblocktx`   | integer | Transaction count in the current block being built            |
| `difficulty`       | number  | Current network difficulty                                    |
| `errors`           | string  | Most recent mining error (empty if none)                      |
| `genproclimit`     | integer | Configured generation processor limit (-1 = no limit)         |
| `localsolps`       | number  | Local Equihash solutions per second (0.0 on non-mining nodes) |
| `networksolps`     | number  | Estimated network Equihash solutions per second               |
| `networkhashps`    | number  | Same as `networksolps` (for Bitcoin-family compatibility)     |
| `pooledtx`         | integer | Number of transactions in the mempool                         |
| `testnet`          | boolean | `true` if serving Testnet                                     |
| `chain`            | string  | Chain name (`main` or `test`)                                 |

## Use Cases

* **Mining Pool Dashboards**: Display current network hashrate, difficulty, and mempool state
* **Miner Configuration Sanity Checks**: Verify node reports correct chain and testnet state
* **Network Statistics**: Feed hashrate estimates into chain-analytics dashboards

## Error Handling

| Error Code | Message        | Description                         |
| ---------- | -------------- | ----------------------------------- |
| -32603     | Internal error | Node failed to compile mining state |
