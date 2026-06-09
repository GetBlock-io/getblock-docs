---
description: >-
  Example code for the tx_pool_info JSON-RPC method. Complete guide on how to
  use tx_pool_info JSON-RPC in GetBlock Web3 documentation.
---

# tx\_pool\_info - Nervos

Returns the statistics of the tx pool — counts of pending, proposed, orphan transactions, total size, fees, and minimum fee rate.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "tx_pool_info",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "tx_pool_info",
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
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "tx_pool_info",
    "params": [],
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
        "method": "tx_pool_info",
        "params": [],
        "id": "getblock.io"
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "last_txs_updated_at": "0x18f3b85f6c8",
        "min_fee_rate": "0x3e8",
        "min_rbf_rate": "0x5dc",
        "orphan": "0x0",
        "pending": "0x42",
        "proposed": "0x12",
        "tip_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
        "tip_number": "0xe5a45f",
        "total_tx_cycles": "0xa3e80000",
        "total_tx_size": "0x4e20",
        "tx_size_limit": "0x9bd02",
        "max_tx_pool_size": "0xb6bd380"
    }
}
```
{% endcode %}

## Response Parameters

| Field                    | Type   | Description                                                               |
| ------------------------ | ------ | ------------------------------------------------------------------------- |
| result.pending           | Uint64 | Number of pending transactions                                            |
| result.proposed          | Uint64 | Number of proposed transactions (committed in the next 2-10 block window) |
| result.orphan            | Uint64 | Number of orphan transactions                                             |
| result.total\_tx\_size   | Uint64 | Total size of all pool transactions in bytes                              |
| result.total\_tx\_cycles | Uint64 | Total cycles of all pool transactions                                     |
| result.min\_fee\_rate    | Uint64 | Minimum fee rate for the pool (Shannon per byte)                          |
| result.tip\_number       | Uint64 | Tip block number the pool is based on                                     |
| result.tip\_hash         | H256   | Tip block hash the pool is based on                                       |

## Use Cases

* Mempool monitoring dashboards
* Fee market analysis
* Detecting network congestion

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| -32603      | Internal error    | Server-side error while processing the request       |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('tx_pool_info', []);
console.log(result);
```
{% endtab %}

{% tab title="ckb-sdk-python / requests" %}
```python
# Using the official CKB SDK Python (ckb-sdk-python).
# For methods not yet wrapped, fall back to raw requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'tx_pool_info',
    'params': []
})
print(response.json())
```
{% endtab %}
{% endtabs %}
