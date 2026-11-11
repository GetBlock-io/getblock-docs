---
description: >-
  Example code for the zks_getAllAccountBalances JSON-RPC method. Complete guide
  on how to use zks_getAllAccountBalances JSON-RPC in GetBlock Web3
  documentation.
---

# zks\_getAllAccountBalances - Lens

Returns every token balance held by an account as a map of token address to balance, in one call — a convenience the standard eth\_\* API does not provide.

## Parameters

| Parameter | Type   | Required | Description     |
| --------- | ------ | -------- | --------------- |
| address   | string | Yes      | Account address |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getAllAccountBalances",
    "params": ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', method: 'zks_getAllAccountBalances', params: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"], id: 'getblock.io' }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'method': 'zks_getAllAccountBalances', 'params': ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"], 'id': 'getblock.io'})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","method":"zks_getAllAccountBalances","params":["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"],"id":"getblock.io"})).send().await?.json::<Value>().await?;
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
        "0x0000000000000000000000000000000000000000": "0x1bc16d674ec80000",
        "0x1e5a153EB7Af838Ad19d56E1571FBD09333C2809": "0xde0b6b3a7640000"
    }
}
```

## Response Fields

| Field | Type   | Description                                    |
| ----- | ------ | ---------------------------------------------- |
|       | string | Balance for each token the account holds (wei) |

## Use Cases

* **Portfolio**: List an account's full holdings
* **Wallets**: Populate a balances view

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
