# starknet\_syncing starknet

This method returns an object describing sync progress while the node is catching up, or false once it is fully synced. It is used to confirm an endpoint is at the chain tip before relying on its data.

## Parameters

{% hint style="info" %}
This method does not require any parameters. Send the request with an empty `params` array.
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
    "method": "starknet_syncing",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'starknet_syncing',
    params: [],
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
        'method': 'starknet_syncing',
        'params': [],
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
            "method": "starknet_syncing",
            "params": [],
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
        "starting_block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4",
        "starting_block_num": "0xa2c2a",
        "current_block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4",
        "current_block_num": "0xab8a0",
        "highest_block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4",
        "highest_block_num": "0xab8a0"
    }
}
```

## Response Parameters

| Parameter | Type              | Description                                                   |
| --------- | ----------------- | ------------------------------------------------------------- |
| jsonrpc   | string            | JSON-RPC protocol version ("2.0")                             |
| id        | string            | Request identifier matching the request                       |
| result    | boolean \| object | false when synced, or a sync-status object (see fields below) |

### Result Object

| Field                | Type   | Description                            |
| -------------------- | ------ | -------------------------------------- |
| starting\_block\_num | string | Block the sync started from (felt)     |
| current\_block\_num  | string | Block currently processed (felt)       |
| highest\_block\_num  | string | Best known block on the network (felt) |

## Use Cases

* **Readiness Gates**: Hold traffic until the node reports it is fully synced
* **Data Trust**: Avoid serving stale reads from a node still catching up
* **Monitoring**: Alert when a node falls behind the network tip
* **Failover Logic**: Route away from nodes reporting an incomplete sync

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| 63 / UNEXPECTED\_ERROR    | Unexpected error | The node failed to report sync status             |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
