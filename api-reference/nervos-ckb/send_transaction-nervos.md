---
description: >-
  Example code for the send_transaction JSON-RPC method. Complete guide on how
  to use send_transaction JSON-RPC in GetBlock Web3 documentation.
---

# send\_transaction - Nervos

Submits a new transaction to the tx pool. Returns the transaction hash. Uses `passthrough` (default) for outputs validator — pass `well_known_scripts_only` to enforce that all outputs use well-known lock scripts (a basic safety check).

## Parameters

| Parameter          | Type        | Required | Description                                          |
| ------------------ | ----------- | -------- | ---------------------------------------------------- |
| tx                 | Transaction | Yes      | Signed transaction                                   |
| outputs\_validator | string      | No       | `passthrough` (default) or `well_known_scripts_only` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "send_transaction",
    "params": [
        {
            "version": "0x0",
            "cell_deps": [],
            "header_deps": [],
            "inputs": [],
            "outputs": [],
            "outputs_data": [],
            "witnesses": []
        },
        "passthrough"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "send_transaction",
    "params": [
        {
            "version": "0x0",
            "cell_deps": [],
            "header_deps": [],
            "inputs": [],
            "outputs": [],
            "outputs_data": [],
            "witnesses": []
        },
        "passthrough"
    ],
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
    "method": "send_transaction",
    "params": [
        {
            "version": "0x0",
            "cell_deps": [],
            "header_deps": [],
            "inputs": [],
            "outputs": [],
            "outputs_data": [],
            "witnesses": []
        },
        "passthrough"
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
        "method": "send_transaction",
        "params": [
                {
                        "version": "0x0",
                        "cell_deps": [],
                        "header_deps": [],
                        "inputs": [],
                        "outputs": [],
                        "outputs_data": [],
                        "witnesses": []
                },
                "passthrough"
        ],
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
    "result": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"
}
```
{% endcode %}

## Response Parameters

| Field  | Type | Description                                                 |
| ------ | ---- | ----------------------------------------------------------- |
| result | H256 | Transaction hash — use `get_transaction` to track inclusion |

## Use Cases

* Wallet transaction submission
* DEX backends submitting orders
* Cross-chain bridge relayer transaction submission

## Error Handling

| Status Code    | Error Message                                   | Cause                                                                                      |
| -------------- | ----------------------------------------------- | ------------------------------------------------------------------------------------------ |
| 403            | Forbidden                                       | Missing or invalid `<ACCESS-TOKEN>`                                                        |
| -32602         | Invalid params                                  | Request parameters are missing or malformed                                                |
| -32601         | Method not found                                | Method does not exist or is not enabled on this node                                       |
| -32603         | Internal error                                  | Server-side error while processing the request                                             |
| 429            | Too Many Requests                               | Rate limit exceeded for your plan                                                          |
| -1000 to -1099 | PoolRejectedTransactionByOutputsValidator       | Transaction rejected by output validator (use `passthrough` if you know what you're doing) |
| -1101          | PoolRejectedTransactionByIllTransactionChecker  | Transaction contains malformed components                                                  |
| -1102          | PoolRejectedTransactionByMinFeeRate             | Fee rate below the minimum                                                                 |
| -1103          | PoolRejectedTransactionByMaxAncestorsCountLimit | Transaction chain exceeds the ancestors limit                                              |
| -1104          | PoolIsFull                                      | The mempool is full                                                                        |
| -1105          | PoolRejectedDuplicatedTransaction               | Transaction is already in the pool                                                         |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('send_transaction', [{"version": "0x0", "cell_deps": [], "header_deps": [], "inputs": [], "outputs": [], "outputs_data": [], "witnesses": []}, "passthrough"]);
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
    'method': 'send_transaction',
    'params': [{"version": "0x0", "cell_deps": [], "header_deps": [], "inputs": [], "outputs": [], "outputs_data": [], "witnesses": []}, "passthrough"]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
