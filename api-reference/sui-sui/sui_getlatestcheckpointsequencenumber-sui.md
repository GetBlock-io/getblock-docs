---
description: >-
  Example code for the sui_getLatestCheckpointSequenceNumber JSON-RPC method.
  Complete guide on how to use sui_getLatestCheckpointSequenceNumber JSON-RPC in
  GetBlock Web3 documentation.
---

# sui\_getLatestCheckpointSequenceNumber - Sui

This method returns the sequence number of the latest checkpoint that has been executed on the SUI network. This provides a quick way to determine the current height of the blockchain without fetching full checkpoint data. It's useful for monitoring network progress and synchronization status.

## Parameters

* None

## Returns

| Field  | Type   | Description                           |
| ------ | ------ | ------------------------------------- |
| result | string | The latest checkpoint sequence number |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_getLatestCheckpointSequenceNumber",
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
  method: 'sui_getLatestCheckpointSequenceNumber',
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
    "method": "sui_getLatestCheckpointSequenceNumber",
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
        "method": "sui_getLatestCheckpointSequenceNumber",
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
  "result": "507021",
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| result    | string | Latest checkpoint sequence number as a string |

## Use Cases

* Monitor blockchain height and progress
* Check node synchronization status
* Implement polling for new checkpoints
* Calculate finality depth for transactions
* Display network status in dashboards

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

const latestCheckpoint = await client.getLatestCheckpointSequenceNumber();

console.log(latestCheckpoint);
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
    
    let seq = sui.read_api().get_latest_checkpoint_sequence_number().await?;
    
    println!("{}", seq);
    Ok(())
}
```
{% endtab %}
{% endtabs %}
