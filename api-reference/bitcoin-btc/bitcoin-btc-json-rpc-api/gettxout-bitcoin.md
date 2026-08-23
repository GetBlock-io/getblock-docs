# gettxout bitcoin

This method returns details about an unspent transaction output (UTXO). If the output has been spent or does not exist, the result is null.

## Parameters

| Parameter        | Type    | Required | Description                                    |
| ---------------- | ------- | -------- | ---------------------------------------------- |
| txid             | string  | Yes      | The transaction ID                             |
| n                | number  | Yes      | The output index (vout)                        |
| include\_mempool | boolean | No       | Whether to include the mempool (default: true) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettxout",
    "params": ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", 0, true],
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
    method: 'gettxout',
    params: ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", 0, true],
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
        'method': 'gettxout',
        'params': ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", 0, true],
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
            "method": "gettxout",
            "params": ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", 0, true],
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
        "bestblock": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
        "confirmations": 152,
        "value": 6.25,
        "scriptPubKey": {
            "asm": "OP_DUP OP_HASH160 ...",
            "hex": "76a914...88ac",
            "type": "pubkeyhash",
            "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
        },
        "coinbase": true
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                |
| id        | string | Request identifier matching the request          |
| result    | object | UTXO object, or null if unspent output not found |

### Result Object

| Field         | Type    | Description                                       |
| ------------- | ------- | ------------------------------------------------- |
| bestblock     | string  | Hash of the block at the chain tip                |
| confirmations | number  | Number of confirmations                           |
| value         | number  | Output value in BTC                               |
| scriptPubKey  | object  | The output script, including type and address     |
| coinbase      | boolean | Whether the output is from a coinbase transaction |

## Use Cases

* **UTXO Checks**: Confirm an output is unspent before spending
* **Balance Building**: Sum UTXOs to compute a balance
* **Coin Selection**: Read output values for input selection
* **Wallets**: Validate outputs during transaction building

## Error Handling

| Error Code | Message           | Description                                      |
| ---------- | ----------------- | ------------------------------------------------ |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                  |
| -5         | Not found         | The requested block or transaction was not found |
| -8         | Invalid parameter | A parameter is out of range or malformed         |
| -32602     | Invalid params    | A parameter is missing or has the wrong type     |
