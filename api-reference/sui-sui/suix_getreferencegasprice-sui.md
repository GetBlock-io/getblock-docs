---
description: >-
  Example code for the suix_getReferenceGasPrice JSON-RPC method. Complete guide
  on how to use suix_getReferenceGasPrice JSON-RPC in GetBlock Web3
  documentation.
---

# suix\_getReferenceGasPrice - Sui

This method returns the reference gas price for the current epoch on the SUI network. This minimum gas price is determined by validator voting and is essential for setting appropriate transaction gas budgets.

### Parameters

* None

### Request Example

{% tabs %}
{% tab title="curl" %}
{% code title="curl" %}
```bash
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/ \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "suix_getReferenceGasPrice",
  "params": [],
  "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
axios.post("https://go.getblock.io/<ACCESS-TOKEN>/", {
  jsonrpc: "2.0", method: "suix_getReferenceGasPrice", params: [], id: "getblock.io"
}).then(r => console.log("Gas Price:", r.data.result));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
payload = {"jsonrpc": "2.0", "method": "suix_getReferenceGasPrice", "params": [], "id": "getblock.io"}
response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print("Gas Price:", response.json().get("result"))
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" overflow="wrap" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let payload = json!({"jsonrpc": "2.0", "id": "getblock.io", "method": "suix_getReferenceGasPrice", "params": []});
    let response = client.post("https://go.getblock.io/<ACCESS-TOKEN>/").json(&payload).send().await?;
    println!("Response: {:?}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Sample

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "1000"
}
```
{% endcode %}

### Response Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| result    | string | Reference gas price in MIST per gas unit |

### Use Cases

1.  Transaction Construction

    Set the appropriate gas price when building transactions.
2.  Cost Estimation

    Calculate expected transaction costs before submission.
3.  Gas Price Monitoring

    Track network gas prices for analytics.

### Error Handling

Network Error

* Description: Connection issues
* Solution: Check endpoint availability

### Web3 Integration

{% code title="Sui Typescript SDK" %}
```jsx
import { SuiClient } from '@mysten/sui/client';
const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const gasPrice = await client.getReferenceGasPrice();
console.log(gasPrice);
```
{% endcode %}
