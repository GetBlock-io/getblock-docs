# unconfirmed\_txs

Returns the unconfirmed transactions currently in the mempool, up to a limit.

## Parameters

| Parameter | Type   | Required | Description                |
| --------- | ------ | -------- | -------------------------- |
| limit     | string | Optional | Max transactions to return |

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
    "method": "unconfirmed_txs",
    "params": {"limit": "30"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'unconfirmed_txs', params: {"limit": "30"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'unconfirmed_txs', 'params': {"limit": "30"}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"unconfirmed_txs","params":{"limit": "30"}})).send().await?.json::<Value>().await?;
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
        "n_txs": "3",
        "total": "3",
        "txs": [
            "Cr0BC..."
        ]
    }
}
```

## Response Fields

| Field  | Type   | Description                 |
| ------ | ------ | --------------------------- |
| txs    | array  | Base64 mempool transactions |
| n\_txs | string | Number returned             |
| total  | string | Total in the mempool        |

## Use Cases

* **Mempool Inspection**: See pending transactions
* **Monitoring**: Watch pool depth

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Failed to read mempool                            |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
