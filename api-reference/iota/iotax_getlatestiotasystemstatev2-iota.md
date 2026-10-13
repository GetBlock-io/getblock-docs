---
description: >-
  Example code for the iotax_getLatestIotaSystemStateV2 JSON-RPC method.
  Complete guide on how to use iotax_getLatestIotaSystemStateV2 JSON-RPC method
  in GetBlock Web3 documentation.
---

# iotax\_getLatestIotaSystemStateV2 - IOTA

Returns the latest on-chain system state on protocol version 5 and above: epoch, protocol version, the active validator set, staking pools, and system parameters.

## Parameters

This method takes no parameters.

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
    "method": "iotax_getLatestIotaSystemStateV2",
    "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iotax_getLatestIotaSystemStateV2', params: [] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iotax_getLatestIotaSystemStateV2', 'params': []})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iotax_getLatestIotaSystemStateV2","params":[]})).send().await?.json::<Value>().await?;
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
        "epoch": "742",
        "protocolVersion": "5",
        "referenceGasPrice": "1000",
        "totalStake": "1500000000000000000",
        "activeValidators": [
            {
                "iotaAddress": "0xval...",
                "name": "Validator One",
                "votingPower": "120",
                "stakingPoolIotaBalance": "80000000000000000"
            }
        ]
    }
}
```

## Response Parameters

| Field                   | Type   | Description                                      |
| ----------------------- | ------ | ------------------------------------------------ |
| jsonrpc                 | string | JSON-RPC version ("2.0")                         |
| id                      | string | Request identifier ("getblock.io")               |
| result.epoch            | string | Current epoch                                    |
| result.activeValidators | array  | Active validator set with stake and voting power |
| result.totalStake       | string | Total staked IOTA                                |

## Use Cases

* **Staking UX**: List validators before delegating
* **Network State**: Read system parameters and committee

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
