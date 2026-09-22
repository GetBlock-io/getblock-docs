# block\_results

Returns the ABCI results and events for every transaction in a block, plus block-level events. The primary source for indexing events.

## Parameters

| Parameter | Type   | Required | Description                   |
| --------- | ------ | -------- | ----------------------------- |
| height    | string | Optional | Block height; omit for latest |

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
    "method": "block_results",
    "params": {"height": "19500000"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'block_results', params: {"height": "19500000"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'block_results', 'params': {"height": "19500000"}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"block_results","params":{"height": "19500000"}})).send().await?.json::<Value>().await?;
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
        "height": "19500000",
        "txs_results": [
            {
                "code": 0,
                "gas_used": "120000",
                "events": [
                    {
                        "type": "transfer",
                        "attributes": [
                            {
                                "key": "amount",
                                "value": "1000000uakt"
                            }
                        ]
                    }
                ]
            }
        ]
    }
}
```

## Response Fields

| Field        | Type   | Description                                    |
| ------------ | ------ | ---------------------------------------------- |
| txs\_results | array  | Per-tx ABCI results with code, gas, and events |
| height       | string | Height the results belong to                   |

## Use Cases

* **Event Indexing**: Drive an indexer from events
* **Failure Detection**: Detect failed txs via code
* **Gas Analytics**: Aggregate gas usage

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Height out of range                               |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
