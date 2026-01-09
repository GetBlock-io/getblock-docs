---
description: >-
  Example code for the suix_getValidatorsApy JSON-RPC method. Complete guide on
  how to use suix_getValidatorsApy JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getValidatorsApy - Sui

This method returns the estimated Annual Percentage Yield (APY) for all validators on the SUI network. This information helps delegators choose validators based on their reward performance and is essential for staking analytics.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_getValidatorsApy",
  "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'suix_getValidatorsApy',
  params: []
};

axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', payload)
.then(response => console.log(response.data));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "suix_getValidatorsApy",
    "params": []
}

response = requests.post("https://go.getblock.io/<ACCESS-TOKEN>/", json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="request.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "suix_getValidatorsApy",
        "params": []
    });
    let response = client.post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .json(&payload).send().await?;
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "result": {
    "apys": [
      {
        "address": "0x3befb84f03a24386492bd3b05b1fd386172eb450e5059ce7df0ea6d9d6cefcaa",
        "apy": 0.0345
      },
      {
        "address": "0x9c76d5157eaa77c41a7bfda8db98a8e8080f7cb53b7313088ed085c73f866f21",
        "apy": 0.0312
      }
    ],
    "epoch": "1001"
  },
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| apys      | array  | Array of {address, apy} objects           |
| address   | string | Validator address                         |
| apy       | number | Estimated APY as decimal (0.0345 = 3.45%) |
| epoch     | string | Epoch of calculation                      |

## Use Cases

* Compare validator performance
* Help users choose validators
* Build staking comparison tools
* Track APY trends over time

## Error Handling

| Error Code | Description                    |
| ---------- | ------------------------------ |
| -32603     | Internal error - node issues   |
| -32000     | Server error - RPC unavailable |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="typescript" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const apys = await client.getValidatorsApy();
console.log(apys);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="rust" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let apys = sui.governance_api().get_validators_apy().await?;
    println!("{:?}", apys);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
