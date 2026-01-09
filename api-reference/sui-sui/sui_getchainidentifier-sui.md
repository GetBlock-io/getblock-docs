---
description: >-
  Example code for the sui_getChainIdentifier JSON-RPC method. Complete guide on
  how to use sui_getChainIdentifier JSON-RPC in GetBlock Web3 documentation.
---

# sui\_getChainIdentifier - Sui

This method returns the chain's genesis checkpoint digest (first four bytes as a hex string). This identifier uniquely distinguishes between SUI networks and is used for transaction signing and protection against replay attacks.

### **Parameters**

* None

### Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
     "method": "sui_getChainIdentifier",
  "params": [],
  "id": "getblock.io"
    }'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
     "method": "sui_getChainIdentifier",
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
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
     "method": "sui_getChainIdentifier",
  "params": [],
  "id": "getblock.io"
    })

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
             "method": "sui_getChainIdentifier",
              "params": [],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **Response**

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "4c78adac"
}
```

### Response  Parameters

| Parameter | Type   | Description                                              |
| --------- | ------ | -------------------------------------------------------- |
| result    | string | 8-character hex string representing the chain identifier |

### Use Cases

1. Network Verification: Verify connection to the correct network (Mainnet vs Testnet) before submitting transactions.
2. Replay Protection: Include in transaction signing to prevent cross-chain replay attacks.
3. Multi-Network Apps: Support multiple SUI networks with automatic detection.

### Error Handling

| Error Code | Description                        |
| ---------- | ---------------------------------- |
| -32602     | Invalid params - malformed input   |
| -32603     | Internal error - simulation failed |
| MoveAbort  | Move execution aborted             |

### SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="example.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';
const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const chainId = await client.getChainIdentifier();
console.log(chainId);
```
{% endcode %}
{% endtab %}
{% endtabs %}

