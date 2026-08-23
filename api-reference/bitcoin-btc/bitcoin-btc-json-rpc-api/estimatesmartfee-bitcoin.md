# estimatesmartfee bitcoin

This method estimates the approximate fee rate per kilobyte needed for a transaction to begin confirmation within a given number of blocks.

## Parameters

| Parameter      | Type   | Required | Description                                                                           |
| -------------- | ------ | -------- | ------------------------------------------------------------------------------------- |
| conf\_target   | number | Yes      | Confirmation target in blocks (1 to 1008)                                             |
| estimate\_mode | string | No       | Fee estimate mode: "unset", "economical", or "conservative" (default: "conservative") |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "estimatesmartfee",
    "params": [6, "economical"],
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
    method: 'estimatesmartfee',
    params: [6, "economical"],
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
        'method': 'estimatesmartfee',
        'params': [6, "economical"],
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
            "method": "estimatesmartfee",
            "params": [6, "economical"],
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
        "feerate": 0.00012345,
        "blocks": 6
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Fee estimate object                     |

### Result Object

| Field   | Type   | Description                                      |
| ------- | ------ | ------------------------------------------------ |
| feerate | number | Estimated fee rate in BTC/kB (1 kB = 1000 bytes) |
| blocks  | number | The block number the estimate is for             |
| errors  | array  | Errors encountered during estimation, if any     |

## Use Cases

* **Dynamic Fees**: Compute a fee rate for a target confirmation time
* **Fee Selection UX**: Offer users fast, medium, and slow options
* **RBF Strategies**: Set fees for replaceable transactions
* **Cost Optimization**: Minimize fees for a desired confirmation window

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN              |
| -1         | Insufficient data | Not enough data to produce an estimate       |
| -8         | Invalid parameter | The confirmation target is out of range      |
| -32602     | Invalid params    | A parameter is missing or has the wrong type |
