# dump\_consensus\_state

Returns a detailed dump of the consensus state including peer round states. For debugging.

## Parameters

{% hint style="info" %}
This method takes an empty `params` object.
{% endhint %}

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
    "method": "dump_consensus_state",
    "params": {}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'dump_consensus_state', params: {} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'dump_consensus_state', 'params': {}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"dump_consensus_state","params":{}})).send().await?.json::<Value>().await?;
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
        "round_state": {
            "height": "19500000"
        },
        "peers": []
    }
}
```

## Response Fields

| Field        | Type   | Description                |
| ------------ | ------ | -------------------------- |
| round\_state | object | Full consensus round state |
| peers        | array  | Per-peer round states      |

## Use Cases

* **Deep Debugging**: Inspect full consensus state
* **Diagnostics**: Investigate liveness issues

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Failed to dump state                              |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
