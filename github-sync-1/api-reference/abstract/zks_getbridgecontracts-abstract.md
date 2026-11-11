---
description: >-
  Example code for the zks_getBridgeContracts JSON-RPC method. Complete guide on
  how to use zks_getBridgeContracts JSON-RPC in GetBlock Web3 documentation.
---

# zks\_getBridgeContracts - Abstract

Returns the L1 and L2 addresses of the default ERC-20 and shared bridge contracts used to move assets between Ethereum and Abstract.

## Parameters

{% hint style="info" %}
This method takes an empty `params` array.
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
    "method": "zks_getBridgeContracts",
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
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', method: 'zks_getBridgeContracts', params: [], id: 'getblock.io' }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'method': 'zks_getBridgeContracts', 'params': [], 'id': 'getblock.io'})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","method":"zks_getBridgeContracts","params":[],"id":"getblock.io"})).send().await?.json::<Value>().await?;
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
        "l1Erc20DefaultBridge": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "l2Erc20DefaultBridge": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "l1SharedDefaultBridge": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "l2SharedDefaultBridge": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
    }
}
```

## Response Fields

| Field                 | Type   | Description              |
| --------------------- | ------ | ------------------------ |
| l1SharedDefaultBridge | string | L1 shared bridge address |
| l2SharedDefaultBridge | string | L2 shared bridge address |

## Use Cases

* **Bridging**: Resolve bridge addresses for deposits/withdrawals
* **Integrations**: Wire up cross-chain transfers

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
