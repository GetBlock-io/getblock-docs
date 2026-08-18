---
description: >-
  Example code for the eth_blobBaseFee JSON RPC method. Сomplete guide on how to
  use eth_blobBaseFee JSON RPC in GetBlock Web3 documentation.
---

# eth\_blobBaseFee - Ethereum

This method returns the current blob base fee in wei, hex-encoded. Introduced with EIP-4844 (Dencun, March 2024), blob transactions carry their data-availability payload separately from calldata and are priced independently via `blobGasPrice`. Post-Fusaka (December 2025) and its BPO forks, blob capacity is 14 target / 21 max blobs per block. Rollups and any application posting blob-carrying transactions read this to price their data-availability spend.

## Parameters

This method takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_blobBaseFee",
    "params": [],
    "id": "getblock.io"
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
    method: 'eth_blobBaseFee',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

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
        'method': 'eth_blobBaseFee',
        'params': [],
        'id': 'getblock.io'
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
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_blobBaseFee",
            "params": [],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Result: {}", response["result"]);
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
    "result": "0x7ad94d",
    "id": "getblock.io"
}
```

## Response Parameters

| Parameter | Type   | Description                                        |
| --------- | ------ | -------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                  |
| `id`      | string | Request identifier matching the request            |
| `result`  | string | Hex-encoded blob base fee in wei per blob gas unit |

## Use Cases

* **L2 Rollup Cost Modeling**: L2 sequencers price blob-posting cost before submitting compressed batches to L1
* **Blob Congestion Detection**: Track the blob base fee to detect L2 batch-posting congestion or blob market spikes
* **Blob Transaction Pricing**: Applications posting EIP-4844 blob transactions compute `maxFeePerBlobGas` bids based on this floor
* **Data Availability Analytics**: Research and dashboards tracking DA spend across the rollup ecosystem

## Error Handling

| Error Code | Message        | Description                                        |
| ---------- | -------------- | -------------------------------------------------- |
| -32603     | Internal error | Node's blob fee tracker failed to produce a result |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const blobBaseFeeHex = await provider.send('eth_blobBaseFee', []);
const blobBaseFee = BigInt(blobBaseFeeHex);
console.log('Blob base fee (wei):', blobBaseFee.toString());
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const blobBaseFee = await client.getBlobBaseFee();
console.log('Blob base fee (wei):', blobBaseFee);
```
{% endcode %}
{% endtab %}
{% endtabs %}
