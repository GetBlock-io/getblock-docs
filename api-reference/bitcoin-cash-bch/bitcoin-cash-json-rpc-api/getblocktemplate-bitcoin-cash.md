---
description: >-
  Example code for the getblocktemplate JSON-RPC method. Complete guide on how
  to use getblocktemplate JSON-RPC in GetBlock Web3 documentation.
---

# getblocktemplate - Bitcoin Cash

This method returns data needed to construct a block to work on, following the getblocktemplate mining protocol. It is used by mining software to assemble candidate blocks.

## Parameters

| Parameter         | Type   | Required | Description                                                        |
| ----------------- | ------ | -------- | ------------------------------------------------------------------ |
| template\_request | object | No       | Object specifying template mode, capabilities, and supported rules |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblocktemplate",
    "params": [{"rules": ["csv", "segwit"]}],
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
  jsonrpc: '2.0', method: 'getblocktemplate',
  params: [{ rules: ['csv', 'segwit'] }], id: 'getblock.io'
});
console.log(data.result.height, data.result.target);
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
        'method': 'getblocktemplate',
        'params': [{"rules": ["csv", "segwit"]}],
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
            "method": "getblocktemplate",
            "params": [{"rules": ["csv", "segwit"]}],
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
        "version": 536870912,
        "previousblockhash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
        "height": 684635,
        "curtime": 1617180999,
        "bits": "170cdf6f",
        "target": "000000000000000000073da8...",
        "coinbasevalue": 625000000,
        "mintime": 1617176374,
        "sizelimit": 32000000,
        "transactions": []
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | object       | Block template object for mining                 |

### Result Object

| Field             | Type    | Description                              |
| ----------------- | ------- | ---------------------------------------- |
| version           | numeric | Block version                            |
| previousblockhash | string  | Hash of the current tip to build on      |
| height            | numeric | Height of the block to be mined          |
| coinbasevalue     | numeric | Maximum coinbase value in satoshis       |
| target            | string  | The proof-of-work target as a hex string |
| transactions      | array   | Transactions to include in the block     |

## Use Cases

* **Mining Software**: Feed block templates into a mining worker
* **Pool Operation**: Assemble candidate blocks for pool participants
* **Coinbase Construction**: Read coinbasevalue to build the coinbase transaction
* **Template Monitoring**: Track template changes as the mempool updates

## Error Handling

| Error Code | Message         | Description                                   |
| ---------- | --------------- | --------------------------------------------- |
| -32700     | Parse error     | Request body is not valid JSON                |
| -32600     | Invalid request | The JSON sent is not a valid request object   |
| -32603     | Internal error  | Node failed to read the requested chain state |
