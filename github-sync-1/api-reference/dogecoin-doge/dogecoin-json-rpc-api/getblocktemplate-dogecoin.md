---
description: >-
  Example code for the getblocktemplate JSON-RPC method. Complete guide on how to use
  getblocktemplate JSON-RPC in GetBlock Web3 documentation.
---

# getblocktemplate - Dogecoin

This method returns the data a miner needs to construct a candidate block. It is used by mining software rather than by wallet or payment integrations.

## Parameters

| Parameter      | Type   | Required | Description                                                                 |
| -------------- | ------ | -------- | ----------------------------------------------------------------------------- |
| template_request | object | No     | Request object with `mode`, `capabilities`, and `rules` fields              |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblocktemplate",
    "params": [{"rules": []}],
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
  jsonrpc: '2.0', method: 'getblocktemplate', params: [{"rules": []}], id: 'getblock.io'
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
        'method': 'getblocktemplate',
        'params': [{"rules": []}],
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
            "method": "getblocktemplate",
            "params": [{"rules": []}],
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
        "version": 6422788,
        "previousblockhash": "7d6f509b16de07844787f97b50778d032eb026a470eb0e0fb5b42a73290e7848",
        "transactions": [],
        "coinbasevalue": 1000000000000,
        "target": "0000000000000019 67ad650000000000000000000000000000000000000000",
        "mintime": 1789703286,
        "curtime": 1789703950,
        "bits": "1967ad65",
        "height": 6378960
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result.version | numeric | Block version to use |
| result.previousblockhash | string | Hash the candidate block must build on |
| result.transactions | array | Transactions to include, each with its own data and fee |
| result.coinbasevalue | numeric | Maximum coinbase value in koinu, subsidy plus fees |
| result.target | string | Proof-of-work target the block must meet |
| result.bits | string | Compact form of the target |
| result.height | numeric | Height of the candidate block |

{% hint style="info" %}
Dogecoin is merge-mined with Litecoin, so production mining uses an AuxPoW workflow rather than solving this template directly. The template is still returned in the standard shape.
{% endhint %}

## Use Cases

* **Mining Software**: Build a candidate block to work on
* **Pool Operation**: Distribute work to connected miners
* **Subsidy Inspection**: Read the current coinbase value

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32700 | Parse error | Request body is not valid JSON |
| -32600 | Invalid request | The JSON sent is not a valid request object |
| -32603 | Internal error | Node failed to read the requested chain state |
