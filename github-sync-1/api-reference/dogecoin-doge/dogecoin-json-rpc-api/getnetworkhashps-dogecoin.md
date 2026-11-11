---
description: >-
  Example code for the getnetworkhashps JSON-RPC method. Complete guide on how to use
  getnetworkhashps JSON-RPC in GetBlock Web3 documentation.
---

# getnetworkhashps - Dogecoin

This method returns the estimated network hashes per second, averaged over a number of recent blocks.

## Parameters

| Parameter | Type    | Required | Description                                                                 |
| --------- | ------- | -------- | ----------------------------------------------------------------------------- |
| nblocks   | numeric | No       | Number of blocks to average over. -1 uses the blocks since the last difficulty change. Default 120 |
| height    | numeric | No       | Estimate at the given height instead of the current tip. Default -1         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
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
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'getnetworkhashps', params: [120, -1], id: 'getblock.io'
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
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
    "result": 1847392847362.517
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result | numeric | Estimated network hash rate in hashes per second |

{% hint style="info" %}
Dogecoin is merge-mined with Litecoin using Scrypt, so this figure reflects combined Scrypt hash rate and is not comparable to a SHA-256 chain's number.
{% endhint %}

## Use Cases

* **Mining Dashboards**: Display the current network hash rate
* **Security Analysis**: Track hash rate trends over time
* **Difficulty Context**: Pair with `getdifficulty` to explain block timing

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32700 | Parse error | Request body is not valid JSON |
| -32600 | Invalid request | The JSON sent is not a valid request object |
| -32603 | Internal error | Node failed to read the requested chain state |
