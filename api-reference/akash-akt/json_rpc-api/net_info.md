# net\_info

Returns the node's peer connections and listening status, including per-peer address and node id.

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
    "method": "net_info",
    "params": {}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'net_info', params: {} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'net_info', 'params': {}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"net_info","params":{}})).send().await?.json::<Value>().await?;
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
        "listening": true,
        "n_peers": "24",
        "peers": [
            {
                "node_id": "abc123",
                "remote_ip": "1.2.3.4"
            }
        ]
    }
}
```

## Response Fields

| Field     | Type    | Description                   |
| --------- | ------- | ----------------------------- |
| n\_peers  | string  | Connected peer count          |
| peers     | array   | Connected peers               |
| listening | boolean | Accepting inbound connections |

## Use Cases

* **Connectivity**: Detect an under-connected node
* **Topology**: Enumerate peers
* **Monitoring**: Alert on low peer count

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Failed to report peers                            |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
