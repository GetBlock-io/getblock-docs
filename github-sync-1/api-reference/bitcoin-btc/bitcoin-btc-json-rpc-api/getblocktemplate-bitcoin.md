---
description: >-
  Example code for the getblocktemplate JSON-RPC method. Complete guide on how
  to use getblocktemplate JSON-RPC in GetBlock Web3 documentation.
---

# getblocktemplate - Bitcoin

This method returns data needed to construct a block to work on, following BIP 22, BIP 23, BIP 9, and BIP 145. It is used by mining software.

## Parameters

| Parameter         | Type   | Required | Description                                            |
| ----------------- | ------ | -------- | ------------------------------------------------------ |
| template\_request | object | No       | A JSON object specifying capabilities, rules, and mode |

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
    "params": [{"rules": ["segwit"]}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'getblocktemplate',
    params: [{"rules": ["segwit"]}],
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
        'method': 'getblocktemplate',
        'params': [{"rules": ["segwit"]}],
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
            "method": "getblocktemplate",
            "params": [{"rules": ["segwit"]}],
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
        "version": 671088644,
        "previousblockhash": "0000...",
        "height": 830001,
        "curtime": 1706886600,
        "bits": "17034219",
        "target": "0000...0000",
        "mintime": 1706883600,
        "sizelimit": 4000000,
        "weightlimit": 4000000,
        "coinbasevalue": 637500000,
        "transactions": [
            {
                "data": "0200...",
                "txid": "...",
                "fee": 2820,
                "weight": 561
            }
        ],
        "longpollid": "0000...123",
        "mutable": [
            "time",
            "transactions",
            "prevblock"
        ],
        "noncerange": "00000000ffffffff"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Block template object                   |

### Result Object

| Field             | Type   | Description                                            |
| ----------------- | ------ | ------------------------------------------------------ |
| previousblockhash | string | Hash of the current tip to build on                    |
| height            | number | Height of the block being built                        |
| coinbasevalue     | number | Maximum coinbase value in satoshis (subsidy plus fees) |
| transactions      | array  | Transactions to include, with data and fees            |
| target            | string | The proof-of-work target                               |

## Use Cases

* **Mining Software**: Construct candidate blocks to mine
* **Pool Operations**: Distribute work to miners
* **Fee Analysis**: Read the fees of template transactions
* **Research**: Study block construction

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN              |
| -8         | Invalid parameter | A parameter is out of range or malformed     |
| -32601     | Method not found  | The method is not available on this endpoint |
| -32602     | Invalid params    | A parameter is missing or has the wrong type |
