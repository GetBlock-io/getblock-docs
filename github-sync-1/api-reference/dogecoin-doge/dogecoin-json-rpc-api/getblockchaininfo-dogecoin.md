---
description: >-
  Example code for the getblockchaininfo JSON-RPC method. Complete guide on how to use
  getblockchaininfo JSON-RPC in GetBlock Web3 documentation.
---

# getblockchaininfo - Dogecoin

This method returns an object containing various state information regarding blockchain processing.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockchaininfo",
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
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'getblockchaininfo', params: [], id: 'getblock.io'
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getblockchaininfo',
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "getblockchaininfo",
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
    "result": {
        "chain": "main",
        "blocks": 6378959,
        "headers": 6378959,
        "bestblockhash": "7d6f509b16de07844787f97b50778d032eb026a470eb0e0fb5b42a73290e7848",
        "difficulty": 25802469.97465503,
        "mediantime": 1789703285,
        "verificationprogress": 0.9999995,
        "chainwork": "000000000000000000000000000000000000000000000f2c7c1f3d5a5f0b9e11",
        "pruned": false,
        "softforks": [],
        "bip9_softforks": {}
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result.chain | string | Network name: `main`, `test`, or `regtest` |
| result.blocks | numeric | Height of the most-work validated chain |
| result.headers | numeric | Height of the best header chain |
| result.bestblockhash | string | Hash of the current chain tip |
| result.difficulty | numeric | Current proof-of-work difficulty |
| result.mediantime | numeric | Median time of the last 11 blocks |
| result.verificationprogress | numeric | Estimate of verification progress, 0 to 1 |
| result.pruned | boolean | True when the node is running in pruned mode |

## Use Cases

* **Sync Gating**: Require `blocks` to equal `headers` before trusting chain reads
* **Network Assertion**: Confirm `chain` is `main` before treating data as mainnet
* **Dashboarding**: Surface height, difficulty, and verification progress
* **Capability Checks**: Detect a pruned node before requesting old blocks

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32700 | Parse error | Request body is not valid JSON |
| -32600 | Invalid request | The JSON sent is not a valid request object |
| -32603 | Internal error | Node failed to read the requested chain state |
