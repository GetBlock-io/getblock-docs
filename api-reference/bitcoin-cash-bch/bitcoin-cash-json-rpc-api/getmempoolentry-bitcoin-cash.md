---
description: >-
  Example code for the getmempoolentry JSON-RPC method. Complete guide on how to
  use the getmempoolentry JSON-RPC method in the GetBlock Web3 documentation.
---

# getmempoolentry - Bitcoin Cash

This method returns mempool data for a single transaction that is currently in the mempool, including its fees, size, and ancestor and descendant counts.

## Parameters

| Parameter | Type   | Required | Description                                      |
| --------- | ------ | -------- | ------------------------------------------------ |
| txid      | string | Yes      | The transaction id, which must be in the mempool |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getmempoolentry",
    "params": ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"],
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
  jsonrpc: '2.0', method: 'getmempoolentry',
  params: ['10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642'], id: 'getblock.io'
});
console.log(data.result.fees);
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
        'method': 'getmempoolentry',
        'params': ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"],
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
            "method": "getmempoolentry",
            "params": ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"],
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
        "size": 226,
        "time": 1617180599,
        "height": 684634,
        "descendantcount": 1,
        "descendantsize": 226,
        "ancestorcount": 1,
        "ancestorsize": 226,
        "fees": {
            "base": 2.26e-06,
            "modified": 2.26e-06,
            "ancestor": 2.26e-06,
            "descendant": 2.26e-06
        }
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | object       | Mempool entry object for the transaction         |

### Result Object

| Field           | Type    | Description                                          |
| --------------- | ------- | ---------------------------------------------------- |
| size            | numeric | Transaction size in bytes                            |
| time            | numeric | Time the transaction entered the mempool             |
| ancestorcount   | numeric | Number of in-mempool ancestor transactions           |
| descendantcount | numeric | Number of in-mempool descendant transactions         |
| fees            | object  | Base, modified, ancestor, and descendant fees in BCH |

## Use Cases

* **Fee Bumping**: Read ancestor fees before deciding on a child-pays-for-parent spend
* **Confirmation Estimates**: Assess a transaction's position by its fee rate
* **Chain Limits**: Check ancestor and descendant counts against mempool limits
* **Stuck Transactions**: Diagnose why a low-fee transaction remains unconfirmed

## Error Handling

| Error Code | Message               | Description                                                              |
| ---------- | --------------------- | ------------------------------------------------------------------------ |
| -8         | Invalid parameter     | txid is not a valid 64-character hex string                              |
| -5         | Transaction not found | No transaction with the given id is available; a txindex may be required |
| -32603     | Internal error        | Node failed to read the transaction                                      |
