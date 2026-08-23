# getmempoolentry bitcoin

This method returns mempool data for a specific transaction that is currently in the memory pool.

## Parameters

| Parameter | Type   | Required | Description        |
| --------- | ------ | -------- | ------------------ |
| txid      | string | Yes      | The transaction ID |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getmempoolentry",
    "params": ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"],
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
    method: 'getmempoolentry',
    params: ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"],
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
        'method': 'getmempoolentry',
        'params': ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"],
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
            "method": "getmempoolentry",
            "params": ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"],
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
        "vsize": 141,
        "weight": 561,
        "time": 1706886100,
        "height": 830000,
        "descendantcount": 1,
        "descendantsize": 141,
        "ancestorcount": 1,
        "ancestorsize": 141,
        "fees": {
            "base": 2.82e-05,
            "modified": 2.82e-05,
            "ancestor": 2.82e-05,
            "descendant": 2.82e-05
        },
        "depends": [],
        "spentby": [],
        "bip125-replaceable": true,
        "unbroadcast": false
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Mempool entry object                    |

### Result Object

| Field              | Type    | Description                                  |
| ------------------ | ------- | -------------------------------------------- |
| vsize              | number  | Virtual transaction size                     |
| fees.base          | number  | Base fee in BTC                              |
| ancestorcount      | number  | Number of in-mempool ancestor transactions   |
| descendantcount    | number  | Number of in-mempool descendant transactions |
| bip125-replaceable | boolean | Whether the transaction signals RBF          |

## Use Cases

* **Fee Analysis**: Read a pending transaction's fee and size
* **RBF Detection**: Check whether a transaction is replaceable
* **CPFP Planning**: Read ancestor and descendant relationships
* **Monitoring**: Track a specific pending transaction

## Error Handling

| Error Code | Message           | Description                                      |
| ---------- | ----------------- | ------------------------------------------------ |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                  |
| -5         | Not found         | The requested block or transaction was not found |
| -8         | Invalid parameter | A parameter is out of range or malformed         |
| -32602     | Invalid params    | A parameter is missing or has the wrong type     |
