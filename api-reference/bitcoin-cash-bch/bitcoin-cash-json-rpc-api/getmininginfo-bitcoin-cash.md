---
description: >-
  Example code for the getmininginfo JSON-RPC method. Complete guide on how to
  use the getmininginfo JSON-RPC method in the GetBlock Web3 documentation.
---

# getmininginfo - Bitcoin Cash

This method returns mining-related information including the current block height, difficulty, network hash rate, and mempool transaction count.

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
    "method": "getmininginfo",
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
  jsonrpc: '2.0', method: 'getmininginfo', params: [], id: 'getblock.io'
});
console.log(data.result.difficulty, data.result.networkhashps);
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
        'method': 'getmininginfo',
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
    "error": null,
    "id": "getblock.io",
    "result": {
        "blocks": 684634,
        "chain": "main",
        "currentblocksize": 1999960,
        "currentblocktx": 6269,
        "difficulty": 303698825886.4642,
        "networkhashps": 2.333876557488516e+18,
        "pooledtx": 6334
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | object       | Object of mining-related information             |

### Result Object

| Field         | Type    | Description                                      |
| ------------- | ------- | ------------------------------------------------ |
| blocks        | numeric | Current block height                             |
| difficulty    | numeric | Current proof-of-work difficulty                 |
| networkhashps | numeric | Estimated network hash rate in hashes per second |
| pooledtx      | numeric | Number of transactions in the mempool            |
| chain         | string  | Network name: main, test, or regtest             |

## Use Cases

* **Hashrate Dashboards**: Display estimated network hash rate on a status page
* **Mining Estimates**: Combine difficulty and hashrate for block-time projections
* **Pool Monitoring**: Track pooled transaction counts alongside difficulty
* **Network Reports**: Publish verified difficulty and hashrate figures

## Error Handling

| Error Code | Message         | Description                                   |
| ---------- | --------------- | --------------------------------------------- |
| -32700     | Parse error     | Request body is not valid JSON                |
| -32600     | Invalid request | The JSON sent is not a valid request object   |
| -32603     | Internal error  | Node failed to read the requested chain state |
