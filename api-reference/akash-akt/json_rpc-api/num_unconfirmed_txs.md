# num\_unconfirmed\_txs

Returns the number and total size of transactions in the mempool, without the transactions themselves.

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
    "method": "num_unconfirmed_txs",
    "params": {}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'num_unconfirmed_txs', params: {} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'num_unconfirmed_txs', 'params': {}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"num_unconfirmed_txs","params":{}})).send().await?.json::<Value>().await?;
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
        "total_bytes": "720"
    }
}
```

## Response Fields

| Field        | Type   | Description                    |
| ------------ | ------ | ------------------------------ |
| n\_txs       | string | Number of mempool transactions |
| total\_bytes | string | Total mempool size in bytes    |

## Use Cases

* **Congestion**: Gauge mempool pressure
* **Monitoring**: Track pool size cheaply

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Failed to read mempool                            |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
