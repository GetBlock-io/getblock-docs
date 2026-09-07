---
description: >-
  Example code for the payment_queryFeeDetails JSON-RPC method. Complete guide
  on how to use payment_queryFeeDetails JSON-RPC in GetBlock Web3 documentation.
---

# payment\_queryFeeDetails - Kusama

Returns the detailed inclusion-fee breakdown for a signed extrinsic: the base fee, length fee, and adjusted weight fee that sum to the inclusion fee.

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                     |
| --------- | ------ | -------- | ------------------------------- |
| extrinsic | string | Yes      | Hex-encoded signed extrinsic    |
| at        | string | Optional | Block hash; omit for the latest |

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
    "method": "payment_queryFeeDetails",
    "params": [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'payment_queryFeeDetails',
    params: [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
]
}, { headers: { 'Content-Type': 'application/json' } });

console.log(response.data.result);
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
        'id': 'getblock.io',
        'method': 'payment_queryFeeDetails',
        'params': [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
]
    }
)

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
    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .json(&json!({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "payment_queryFeeDetails",
            "params": [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
]
        }))
        .send().await?
        .json::<Value>().await?;
    println!("{}", response["result"]);
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
        "inclusionFee": {
            "baseFee": "1000000000",
            "lenFee": "160000000",
            "adjustedWeightFee": "440000000"
        }
    }
}
```

## Response Fields

| Field                          | Type   | Description                              |
| ------------------------------ | ------ | ---------------------------------------- |
| inclusionFee.baseFee           | string | Fixed base fee in Planck                 |
| inclusionFee.lenFee            | string | Fee for the extrinsic byte length        |
| inclusionFee.adjustedWeightFee | string | Fee for dispatch weight after adjustment |

## Use Cases

* **Fee Transparency**: Break a fee into its components
* **Optimization**: See how length vs weight drive the fee
* **Accounting**: Record detailed fee data

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| -32603 / Internal error   | Bad extrinsic | The extrinsic could not be decoded                |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
