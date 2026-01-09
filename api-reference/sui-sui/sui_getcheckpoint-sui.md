---
description: >-
  Example code for the sui_getCheckpoint JSON-RPC method. Complete guide on how
  to use sui_getCheckpoint JSON-RPC in GetBlock Web3 documentation.
---

# sui\_getCheckpoint - Sui

This method returns checkpoint information for a specified checkpoint ID or sequence number. Checkpoints are certified snapshots of blockchain state fundamental to SUI's consensus mechanism.

### Parameters

| Parameter | Type   | Required | Description                          | Default/Supported Values    |
| --------- | ------ | -------- | ------------------------------------ | --------------------------- |
| id        | string | Yes      | Checkpoint digest or sequence number | Valid checkpoint identifier |

Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/ \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "sui_getCheckpoint",
  "params": ["1000"],
  "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
payload = {"jsonrpc": "2.0", "method": "sui_getCheckpoint", "params": ["1000"], "id": "getblock.io"}
response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print("Result:", response.json().get("result"))
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
axios.post("https://go.getblock.io/<ACCESS-TOKEN>/", {
  jsonrpc: "2.0", method: "sui_getCheckpoint", params: ["1000"], id: "getblock.io"
}).then(r => console.log("Result:", r.data.result));
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let payload = json!({"jsonrpc": "2.0", "id": "getblock.io", "method": "sui_getCheckpoint", "params": ["1000"]});
    let response = client.post("https://go.getblock.io/<ACCESS-TOKEN>/").json(&payload).send().await?;
    println!("Response: {:?}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "epoch": "5000",
    "sequenceNumber": "1000",
    "digest": "G6Dtzr1ZSfHFhotGsTE3cLENa7L1ooe1BBvknAUsARbV",
    "networkTotalTransactions": "792385",
    "previousDigest": "6tBy8RXZKrdrB4XkMQn7J3MNG4fQCo9XcRduFFvYrL5Z",
    "timestampMs": "1676911928",
    "transactions": ["mN8YNBgVR3wB7vfXmjVgDRF4oqxVRRjzmJ6U4mzbq77"],
    "validatorSignature": "wAAA..."
  }
}
```
{% endcode %}

### Reponse Parameters

| Parameter                       | Type   | Description                  |
| ------------------------------- | ------ | ---------------------------- |
| result.epoch                    | string | Epoch number                 |
| result.sequenceNumber           | string | Checkpoint sequence number   |
| result.digest                   | string | Checkpoint digest            |
| result.networkTotalTransactions | string | Cumulative transaction count |
| result.timestampMs              | string | Checkpoint timestamp         |
| result.transactions             | array  | Transaction digests          |

### Use Cases

1. Block Explorer: Display checkpoint details and included transactions.
2. Finality Verification: Confirm transactions are finalized in checkpoints.
3. Network Sync: Track and verify blockchain state synchronization.

### Common Errors

| Error    | Description               | Solution             |
| -------- | ------------------------- | -------------------- |
| NotFound | Checkpoint does not exist | Verify checkpoint ID |

### SDK Integration

{% tabs %}
{% tab title="Sui Typescript SDK" %}
{% code title="TypeScript (Sui SDK)" %}
```typescript
import { SuiClient } from '@mysten/sui/client';
const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const checkpoint = await client.getCheckpoint({ id: '1000' });
console.log(checkpoint);
```
{% endcode %}
{% endtab %}
{% endtabs %}

