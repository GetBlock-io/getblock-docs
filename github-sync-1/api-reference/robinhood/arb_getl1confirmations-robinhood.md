---
description: >-
  Example code for the arb_getL1Confirmations JSON-RPC method. Complete guide to
  using the arb_getL1Confirmations JSON-RPC method in GetBlock.io Web3
  documentation.
---

# arb\_getL1Confirmations - Robinhood

Returns the number of L1 (Ethereum) block confirmations for a given L2 block hash. This is an Arbitrum-Orbit-canonical method for tracking how deeply an L2 block has been settled on L1. A high confirmation count indicates the block is unlikely to be reorged, providing stronger settlement guarantees than the `finalized` tag on the L2 side alone.

{% hint style="info" %}
**Arbitrum-Orbit-specific method.** This method is part of the Arbitrum Nitro namespace and is not available on generic EVM chains. It exposes L1 settlement information specific to Arbitrum Orbit L2s. `ethers.js` and `viem` do not provide typed wrappers — use the raw request interface as shown in the SDK Integration tabs.
{% endhint %}

## Parameters

| Parameter | Type            | Required | Description                                 |
| --------- | --------------- | -------- | ------------------------------------------- |
| blockHash | DATA (32 bytes) | Yes      | L2 block hash to check L1 confirmations for |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "arb_getL1Confirmations",
    "params": [
        "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "arb_getL1Confirmations",
    "params": [
        "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c"
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/',
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

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "arb_getL1Confirmations",
    "params": [
        "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c"
    ],
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
        "method": "arb_getL1Confirmations",
        "params": [
                "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c"
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/")
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
    "result": "0x12"
}
```
{% endcode %}

## Response Parameters

| Field  | Type     | Description                                                                             |
| ------ | -------- | --------------------------------------------------------------------------------------- |
| result | QUANTITY | Number of L1 block confirmations (hex). `0x0` if the block hasn't been posted to L1 yet |

## Use Cases

* Bridging and withdrawal flows that require confirmed L1 settlement
* High-value transaction settlement verification
* Cross-chain messaging systems that need to wait for L1 finality before acting
* Auditing tools tracking L1 posting cadence

## Error Handling

| Status Code | Error Message      | Cause                                                                 |
| ----------- | ------------------ | --------------------------------------------------------------------- |
| 403         | Forbidden          | Missing or invalid `<ACCESS-TOKEN>`                                   |
| -32602      | Invalid params     | Request parameters are missing or malformed                           |
| -32603      | Internal error     | Server-side error while processing the request                        |
| 429         | Too Many Requests  | Rate limit exceeded for your plan                                     |
| -32602      | Invalid block hash | Block hash is not a valid 32-byte hex string, or block does not exist |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

// Arbitrum-Orbit-specific methods don't have ethers wrappers — use .send():
const result = await provider.send('arb_getL1Confirmations', ["0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c"]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/')
});

// Arbitrum-Orbit-specific methods need the raw request transport:
const result = await client.request({
    method: 'arb_getL1Confirmations',
    params: ["0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c"]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
