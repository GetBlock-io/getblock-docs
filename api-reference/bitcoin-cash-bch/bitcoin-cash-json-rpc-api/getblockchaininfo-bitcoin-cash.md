---
description: >-
  Example code for the getblockchaininfo JSON-RPC method. Complete guide on how
  to use getblockchaininfo JSON-RPC in GetBlock Web3 documentation.
---

# getblockchaininfo - Bitcoin Cash

This method returns state information about the blockchain the node is connected to, including chain name, height, best block hash, and verification progress.

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
const { data } = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'getblockchaininfo', params: [], id: 'getblock.io'
});
console.log(data.result.chain, data.result.blocks);
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
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
        "blocks": 684634,
        "headers": 684634,
        "bestblockhash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
        "difficulty": 303127737690.0432,
        "mediantime": 1619163231,
        "verificationprogress": 0.9999847435404801,
        "initialblockdownload": false,
        "chainwork": "0000000000000000000000000000000000000000016623d7bc4511ec504acf5b",
        "size_on_disk": 183446378110,
        "pruned": false
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | object       | Object describing the current blockchain state   |

### Result Object

| Field                | Type    | Description                                |
| -------------------- | ------- | ------------------------------------------ |
| chain                | string  | Network name: main, test, or regtest       |
| blocks               | numeric | Height of the most-work validated chain    |
| headers              | numeric | Number of headers validated                |
| bestblockhash        | string  | Hash of the current chain tip              |
| verificationprogress | numeric | Estimate of verification progress, 0 to 1  |
| pruned               | boolean | Whether the node is running in pruned mode |

## Use Cases

* **Sync Status**: Report whether the node has finished initial block download
* **Network Detection**: Confirm the node is on mainnet before broadcasting
* **Health Dashboards**: Surface height and verification progress together
* **Storage Planning**: Read size\_on\_disk to monitor node disk usage

## Error Handling

| Error Code | Message         | Description                                   |
| ---------- | --------------- | --------------------------------------------- |
| -32700     | Parse error     | Request body is not valid JSON                |
| -32600     | Invalid request | The JSON sent is not a valid request object   |
| -32603     | Internal error  | Node failed to read the requested chain state |
