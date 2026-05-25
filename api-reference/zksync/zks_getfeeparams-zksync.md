---
description: >-
  Example code for the zks_getFeeParams JSON-RPC method. Сomplete guide on how
  to use zks_getFeeParams JSON-RPC in GetBlock.io Web3 documentation.
---

# zks\_getFeeParams - zkSync

Returns the current fee parameters used by the zkSync Era node — including L1 gas price, pubdata price, compute and pubdata overhead, and batch-level gas limits. These parameters drive the fee model.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getFeeParams",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "zks_getFeeParams",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "zks_getFeeParams",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "zks_getFeeParams",
        "params": [],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "V2": {
            "config": {
                "minimal_l2_gas_price": 45250000,
                "compute_overhead_part": 0.0,
                "pubdata_overhead_part": 1.0,
                "batch_overhead_l1_gas": 800000,
                "max_gas_per_batch": 200000000,
                "max_pubdata_per_batch": 500000
            },
            "l1_gas_price": 152278857,
            "l1_pubdata_price": 7859874,
            "conversion_ratio": {
                "l1": {
                    "numerator": 1,
                    "denominator": 1
                },
                "sl": {
                    "numerator": 1,
                    "denominator": 1
                },
                "numerator": 1,
                "denominator": 1
            }
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                                     | Type    | Description                               |
| ----------------------------------------- | ------- | ----------------------------------------- |
| result.V2.config.minimal\_l2\_gas\_price  | integer | Minimal gas price on L2 (wei)             |
| result.V2.config.compute\_overhead\_part  | number  | Compute overhead share in fee calculation |
| result.V2.config.pubdata\_overhead\_part  | number  | Pubdata overhead share in fee calculation |
| result.V2.config.batch\_overhead\_l1\_gas | integer | L1 gas overhead per batch                 |
| result.V2.config.max\_gas\_per\_batch     | integer | Maximum gas allowed per batch             |
| result.V2.config.max\_pubdata\_per\_batch | integer | Maximum pubdata allowed per batch         |
| result.V2.l1\_gas\_price                  | integer | Current L1 gas price                      |
| result.V2.l1\_pubdata\_price              | integer | Current price of storing pubdata on L1    |

## Use Cases

* Building accurate fee oracles for zkSync transactions
* Predicting transaction costs before broadcast
* Auditing fee model parameters over time

## Error Handling

| Status Code | Error Message     | Cause                               |
| ----------- | ----------------- | ----------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>` |
| 429         | Too Many Requests | Rate limit exceeded for your plan   |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('zks_getFeeParams', []);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'zks_getFeeParams'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('zks_getFeeParams', [])
print(result)
```
{% endtab %}
{% endtabs %}
