---
description: >-
  Example code for the testmempoolaccept JSON-RPC method. Complete guide on how
  to use testmempoolaccept JSON-RPC in GetBlock Web3 documentation.
---

# testmempoolaccept - Bitcoin

This method checks whether one or more raw transactions would be accepted into the mempool, without broadcasting them. It is used to validate a transaction before sending it.

## Parameters

| Parameter  | Type             | Required | Description                                                       |
| ---------- | ---------------- | -------- | ----------------------------------------------------------------- |
| rawtxs     | array            | Yes      | Raw transactions as hex strings (currently one at a time)         |
| maxfeerate | number or string | No       | Reject if the fee rate exceeds this value (default: 0.10 BTC/kvB) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "testmempoolaccept",
    "params": [["0200000001a1b2c3d4e5f6...0000000000"]],
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
    method: 'testmempoolaccept',
    params: [["0200000001a1b2c3d4e5f6...0000000000"]],
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
        'method': 'testmempoolaccept',
        'params': [["0200000001a1b2c3d4e5f6...0000000000"]],
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
            "method": "testmempoolaccept",
            "params": [["0200000001a1b2c3d4e5f6...0000000000"]],
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
    "result": [
        {
            "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
            "wtxid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
            "allowed": true,
            "vsize": 141,
            "fees": {
                "base": 2.82e-05,
                "effective-feerate": 0.0002,
                "effective-includes": [
                    "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"
                ]
            }
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                |
| id        | string | Request identifier matching the request          |
| result    | array  | Array with one acceptance result per transaction |

### Array Item

| Field         | Type    | Description                                    |
| ------------- | ------- | ---------------------------------------------- |
| txid          | string  | The transaction ID                             |
| allowed       | boolean | Whether the transaction would be accepted      |
| vsize         | number  | Virtual size if allowed                        |
| reject-reason | string  | Reason for rejection, present when not allowed |

## Use Cases

* **Pre-Flight Checks**: Validate a transaction before broadcasting
* **Fee Validation**: Confirm the fee rate is acceptable
* **Wallet UX**: Warn users of likely rejection
* **Automation**: Gate broadcasts on acceptance

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN              |
| -8         | Invalid parameter | A parameter is out of range or malformed     |
| -32601     | Method not found  | The method is not available on this endpoint |
| -32602     | Invalid params    | A parameter is missing or has the wrong type |
