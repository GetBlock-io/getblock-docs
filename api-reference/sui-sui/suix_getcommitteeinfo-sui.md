---
description: >-
  Example code for the suix_getCommitteeInfo JSON-RPC method. Complete guide on
  how to use suix_getCommitteeInfo JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getCommitteeInfo - Sui

This method returns the committee information for a specified epoch on the SUI network. The committee consists of validators who participate in consensus for that epoch, along with their voting power (stake weight). This is useful for understanding network decentralization and validator distribution.

## Parameters

| Parameter | Type                | Required | Description                              |
| --------- | ------------------- | -------- | ---------------------------------------- |
| epoch     | BigInt\_for\_uint64 | No       | Epoch number (defaults to current epoch) |

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
  "method": "suix_getCommitteeInfo",
  "params": ["5000"]
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
  method: 'suix_getCommitteeInfo',
  params: ['5000']
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
    "method": "suix_getCommitteeInfo",
    "params": ["5000"]
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
        "method": "suix_getCommitteeInfo",
        "params": ["5000"]
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

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "result": {
    "epoch": "5000",
    "validators": [
      ["jc/20VUECmVvSBmxMRG1LFdGqGunLzlfuSOYGNLSNKY=", "2500000000000000"],
      ["DgDsdbrNH0oq6JHFXVBXHZFQ1XtSNc6bx8v2T8PvhAM=", "1800000000000000"]
    ]
  },
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter  | Type   | Description                               |
| ---------- | ------ | ----------------------------------------- |
| epoch      | string | Epoch number                              |
| validators | array  | Array of \[publicKey, stakeWeight] tuples |

## Use Cases

* Analyze validator stake distribution
* Monitor network decentralization
* Track validator participation history
* Build governance dashboards

## Error Handling

| Error Code | Description                    |
| ---------- | ------------------------------ |
| -32602     | Invalid params - invalid epoch |
| -32603     | Internal error - node issues   |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="sdk.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const committee = await client.getCommitteeInfo({ epoch: '5000' });
console.log(committee);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="sdk.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let committee = sui.governance_api().get_committee_info(Some(5000)).await?;
    println!("{:?}", committee);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
