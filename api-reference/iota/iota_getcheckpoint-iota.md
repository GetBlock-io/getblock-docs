---
description: >-
  Example code for the iota_getCheckpoint JSON-RPC method. Complete guide on how
  to use iota_getCheckpoint JSON-RPC method in GetBlock Web3 documentation.
---

# iota\_getCheckpoint - IOTA

Returns a checkpoint by its sequence number or digest, including its timestamp, the transaction digests it contains, and its epoch.

## Parameters

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
| id        | string | Yes      | Checkpoint sequence number or base58 digest |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "iota_getCheckpoint",
    "params": ["128344567"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_getCheckpoint', params: ["128344567"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_getCheckpoint', 'params': ["128344567"]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_getCheckpoint","params":["128344567"]})).send().await?.json::<Value>().await?;
    println!("{}", res["result"]);
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
        "epoch": "742",
        "sequenceNumber": "128344567",
        "digest": "7Md...",
        "networkTotalTransactions": "2850000000",
        "timestampMs": "1736772000000",
        "transactions": [
            "AdE...",
            "9xK..."
        ]
    }
}
```

## Response Parameters

| Field                 | Type   | Description                           |
| --------------------- | ------ | ------------------------------------- |
| jsonrpc               | string | JSON-RPC version ("2.0")              |
| id                    | string | Request identifier ("getblock.io")    |
| result.sequenceNumber | string | Checkpoint sequence number            |
| result.digest         | string | Checkpoint digest (base58)            |
| result.timestampMs    | string | Checkpoint time (ms since epoch)      |
| result.transactions   | array  | Transaction digests in the checkpoint |

## Use Cases

* **Indexing**: Ingest checkpoints to build an explorer
* **Finality**: Confirm a transaction's checkpoint

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
