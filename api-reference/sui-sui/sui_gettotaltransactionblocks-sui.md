---
description: >-
  Example code for the sui_getTotalTransactionBlocks JSON-RPC method. Complete
  guide on how to use sui_getTotalTransactionBlocks JSON-RPC in GetBlock Web3
  documentation.
---

# sui\_getTotalTransactionBlocks - Sui

This method returns the total number of transactions known to the server on the SUI network. This cumulative count includes all transactions processed since genesis and provides a key metric for network activity and throughput analysis.

## Parameters

* None

## Returns

| Field  | Type   | Description                        |
| ------ | ------ | ---------------------------------- |
| result | string | Total number of transaction blocks |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_getTotalTransactionBlocks",
  "params": []
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'sui_getTotalTransactionBlocks',
  params: []
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "sui_getTotalTransactionBlocks",
    "params": []
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "sui_getTotalTransactionBlocks",
        "params": []
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "4638299150"
}
```

## Response Parameters

| Parameter | Type   | Description                         |
| --------- | ------ | ----------------------------------- |
| result    | string | Total transaction count as a string |

## Use Cases

* Display network statistics in dashboards
* Calculate transaction throughput over time
* Monitor network growth and adoption
* Compare activity across epochs
* Build analytics and reporting tools

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32603     | Internal error - node processing issues |
| -32000     | Server error - RPC endpoint unavailable |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const totalTx = await client.getTotalTransactionBlocks();

console.log(totalTx);
```
{% endtab %}

{% tab title="Sui Rust SDK" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    let total = sui.read_api().get_total_transaction_blocks().await?;
    
    println!("{}", total);
    Ok(())
}
```
{% endtab %}
{% endtabs %}
