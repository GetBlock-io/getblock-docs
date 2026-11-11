---
description: >-
  Example code for the zks_getFeeParams JSON-RPC method. Complete guide on how
  to use zks_getFeeParams JSON-RPC in GetBlock Web3 documentation.
---

# zks\_getFeeParams - Abstract

Returns the current fee model parameters, including the minimal L2 gas price and the pubdata pricing configuration used by the ZK fee model.

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
    "method": "zks_getFeeParams",
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
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', method: 'zks_getFeeParams', params: [], id: 'getblock.io' }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'method': 'zks_getFeeParams', 'params': [], 'id': 'getblock.io'})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","method":"zks_getFeeParams","params":[],"id":"getblock.io"})).send().await?.json::<Value>().await?;
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
        "V2": {
            "config": {
                "minimal_l2_gas_price": "0x17d7840",
                "compute_overhead_part": 0,
                "pubdata_overhead_part": 1,
                "batch_overhead_l1_gas": 800000,
                "max_gas_per_batch": 200000000,
                "max_pubdata_per_batch": 500000
            },
            "l1_gas_price": "0x3b9aca00",
            "l1_pubdata_price": "0x5f5e100"
        }
    }
}
```

## Response Fields

| Field                 | Type   | Description                                                   |
| --------------------- | ------ | ------------------------------------------------------------- |
| V2.config             | object | Fee model configuration (gas prices, overheads, batch limits) |
| V2.l1\_gas\_price     | string | L1 gas price component                                        |
| V2.l1\_pubdata\_price | string | L1 pubdata price component                                    |

## Use Cases

* **Fee Modelling**: Read the parameters behind fee estimates
* **Analytics**: Study fee configuration

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
