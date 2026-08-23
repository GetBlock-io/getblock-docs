# getrawtransaction bitcoin

This method returns the raw transaction data for a given transaction ID. With verbose=true it returns a decoded JSON object; otherwise it returns the serialized hex.

## Parameters

| Parameter | Type    | Required | Description                                               |
| --------- | ------- | -------- | --------------------------------------------------------- |
| txid      | string  | Yes      | The transaction ID                                        |
| verbose   | boolean | No       | true for a decoded object, false for hex (default: false) |
| blockhash | string  | No       | The block to look in, for non-indexed lookups             |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getrawtransaction",
    "params": ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", true],
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
    method: 'getrawtransaction',
    params: ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", true],
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
        'method': 'getrawtransaction',
        'params': ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", true],
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
            "method": "getrawtransaction",
            "params": ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", true],
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
        "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
        "hash": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
        "version": 2,
        "size": 226,
        "vsize": 144,
        "weight": 573,
        "locktime": 0,
        "vin": [
            {
                "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
                "vout": 0,
                "scriptSig": {
                    "asm": "",
                    "hex": ""
                },
                "sequence": 4294967295
            }
        ],
        "vout": [
            {
                "value": 6.25,
                "n": 0,
                "scriptPubKey": {
                    "asm": "OP_DUP OP_HASH160 ...",
                    "hex": "76a914d0f172a0ecb48aee1be1f2687d2963ae33f71a1088ac",
                    "type": "pubkeyhash",
                    "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
                }
            }
        ],
        "hex": "0200000001a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d60000000000ffffffff0100e1f505000000001600141d0f172a0ecb48aee1be1f2687d2963ae33f71a100000000",
        "blockhash": "0000...",
        "confirmations": 152,
        "time": 1706886000,
        "blocktime": 1706886000
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                               |
| --------- | ------ | --------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                         |
| id        | string | Request identifier matching the request                   |
| result    | object | Decoded transaction object (or hex when verbose is false) |

### Result Object

| Field         | Type   | Description                                 |
| ------------- | ------ | ------------------------------------------- |
| txid          | string | The transaction ID                          |
| vin           | array  | Transaction inputs                          |
| vout          | array  | Transaction outputs with values and scripts |
| confirmations | number | Number of confirmations                     |
| hex           | string | Serialized transaction hex                  |

## Use Cases

* **Transaction Views**: Render a transaction's inputs and outputs
* **Payment Verification**: Confirm an output's value and address
* **Indexing**: Ingest and decode transactions
* **Auditing**: Inspect raw transaction data

## Error Handling

| Error Code | Message           | Description                                                     |
| ---------- | ----------------- | --------------------------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                                 |
| -5         | Not found         | No such transaction, or txindex not enabled without a blockhash |
| -8         | Invalid parameter | A parameter is out of range or malformed                        |
| -32602     | Invalid params    | A parameter is missing or has the wrong type                    |
