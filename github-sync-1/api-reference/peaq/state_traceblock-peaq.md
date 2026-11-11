---
description: >-
  Example code for the state_traceBlock JSON-RPC method. Complete guide on how
  to use state_traceBlock JSON-RPC in GetBlock Web3 documentation.
---

# state\_traceBlock - Peaq

Re-executes a block and traces storage accesses for the given targets and keys. Dedicated-node operation.

## Parameters

| # | Example       | Description            |
| - | ------------- | ---------------------- |
| 0 | `0xblockhash` | Positional parameter 0 |
| 1 | `state`       | Positional parameter 1 |
| 2 | `0x26aa`      | Positional parameter 2 |
| 3 | \`\`          | Positional parameter 3 |

## Request

\{% tabs %\} \{% tab title="cURL" %\}

```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "state_traceBlock",
    "params": ["0xblockhash", "state", "0x26aa", ""]
}'
```

{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'state_traceBlock', params: ["0xblockhash", "state", "0x26aa", ""] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}

{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'state_traceBlock', 'params': ["0xblockhash", "state", "0x26aa", ""]})
print(response.json())
```
{% endcode %}

{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let res = client.post("https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"state_traceBlock","params":["0xblockhash", "state", "0x26aa", ""]})).send().await?.json::<Value>().await?;
    println!("{}", res["result"]);
    Ok(())
}
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "blockHash": "0xblockhash",
        "tracingTargets": "state",
        "events": []
    }
}
```

## Response Parameters

| Field   | Type   | Description                           |
| ------- | ------ | ------------------------------------- |
| jsonrpc | string | JSON-RPC version ("2.0")              |
| id      | string | Request identifier ("getblock.io")    |
| result  | varies | The method result (see example above) |

## Use Cases

* Programmatic access to the `state_traceBlock` method on peaq
* Integrations, indexing, and tooling built on the peaq node API

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is not exposed by the node        |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
