---
description: >-
  Example code for the zenlinkProtocol_getEstimateLptoken JSON-RPC method.
  Complete guide on how to use zenlinkProtocol_getEstimateLptoken JSON-RPC in
  GetBlock Web3 documentation.
---

# zenlinkProtocol\_getEstimateLptoken - Peaq

Estimates the LP tokens minted for adding the given amounts of two assets to a Zenlink pool.

## Parameters

| # | Example                                              | Description            |
| - | ---------------------------------------------------- | ---------------------- |
| 0 | `{"chainId": 2241, "assetType": 0, "assetIndex": 0}` | Positional parameter 0 |
| 1 | `{"chainId": 2241, "assetType": 2, "assetIndex": 1}` | Positional parameter 1 |
| 2 | `1000000`                                            | Positional parameter 2 |
| 3 | `1000000`                                            | Positional parameter 3 |
| 4 | `0`                                                  | Positional parameter 4 |
| 5 | `0`                                                  | Positional parameter 5 |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "zenlinkProtocol_getEstimateLptoken",
    "params": [{"chainId": 2241, "assetType": 0, "assetIndex": 0}, {"chainId": 2241, "assetType": 2, "assetIndex": 1}, "1000000", "1000000", "0", "0"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'zenlinkProtocol_getEstimateLptoken', params: [{"chainId": 2241, "assetType": 0, "assetIndex": 0}, {"chainId": 2241, "assetType": 2, "assetIndex": 1}, "1000000", "1000000", "0", "0"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'zenlinkProtocol_getEstimateLptoken', 'params': [{"chainId": 2241, "assetType": 0, "assetIndex": 0}, {"chainId": 2241, "assetType": 2, "assetIndex": 1}, "1000000", "1000000", "0", "0"]})
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
    let res = client.post("https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"zenlinkProtocol_getEstimateLptoken","params":[{"chainId": 2241, "assetType": 0, "assetIndex": 0}, {"chainId": 2241, "assetType": 2, "assetIndex": 1}, "1000000", "1000000", "0", "0"]})).send().await?.json::<Value>().await?;
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
    "result": "999000"
}
```

## Response Parameters

| Field   | Type   | Description                           |
| ------- | ------ | ------------------------------------- |
| jsonrpc | string | JSON-RPC version ("2.0")              |
| id      | string | Request identifier ("getblock.io")    |
| result  | varies | The method result (see example above) |

## Use Cases

* Programmatic access to the `zenlinkProtocol_getEstimateLptoken` method on peaq
* Integrations, indexing, and tooling built on the peaq node API

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is not exposed by the node        |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
