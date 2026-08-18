---
description: >-
  Example code for the getInflationGovernor JSON-RPC method. Complete guide on
  how to use getInflationGovernor JSON-RPC in GetBlock Web3 documentation.
---

# getInflationGovernor - Solana

This method returns the inflation parameters governing SOL issuance, including the initial and terminal rates and the annual taper. These values are set by cluster genesis configuration.

## Parameters

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
| config    | object | No       | Configuration object controlling commitment |

### Config Object

| Field      | Type   | Required | Description                                                                 |
| ---------- | ------ | -------- | --------------------------------------------------------------------------- |
| commitment | string | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getInflationGovernor",
    "params": [{"commitment": "finalized"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'finalized');

const governor = await connection.getInflationGovernor();

console.log(governor);
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
        'method': 'getInflationGovernor',
        'params': [{"commitment": "finalized"}],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
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
            "method": "getInflationGovernor",
            "params": [{"commitment": "finalized"}],
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
        "foundation": 0.0,
        "foundationTerm": 0.0,
        "initial": 0.08,
        "taper": 0.15,
        "terminal": 0.015
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                |
| --------- | ------ | ---------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                          |
| id        | string | Request identifier matching the request                    |
| result    | object | Object holding the cluster's inflation governor parameters |

### Result Object

| Field          | Type   | Description                                              |
| -------------- | ------ | -------------------------------------------------------- |
| initial        | number | Initial annual inflation rate at genesis                 |
| terminal       | number | Long-run annual inflation rate the schedule converges to |
| taper          | number | Annual rate at which inflation decreases toward terminal |
| foundation     | number | Share of total inflation allocated to the foundation     |
| foundationTerm | number | Duration in years of the foundation allocation           |

## Use Cases

* **Yield Modelling**: Project staking returns across future epochs from the taper schedule
* **Tokenomics Research**: Document issuance policy in protocol or investor reporting
* **Dilution Estimates**: Model supply growth against a fixed holding position
* **Cluster Comparison**: Confirm Devnet and Mainnet share the same genesis parameters

## Error Handling

| Error Code | Message        | Description                                              |
| ---------- | -------------- | -------------------------------------------------------- |
| -32602     | Invalid params | Unrecognized commitment level in the config object       |
| -32603     | Internal error | Node failed to read the inflation governor from the bank |
