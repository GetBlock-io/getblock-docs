---
description: >-
  Example code for the estimate_cycle JSON-RPC method. Complete guide on how to
  use estimate_cycle JSON-RPC in GetBlock Web3 documentation.
---

# estimate\_cycle - Nervos

Returns the estimated CKB-VM cycles needed to execute a transaction's scripts. Like Ethereum's `eth_estimateGas` but for CKB-VM cycles instead of gas. The transaction does not have to be valid for execution at the moment of the call.

## Parameters

| Parameter | Type        | Required | Description                        |
| --------- | ----------- | -------- | ---------------------------------- |
| tx        | Transaction | Yes      | Transaction to estimate cycles for |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "estimate_cycles",
    "params": [
        {
            "version": "0x0",
            "cell_deps": [],
            "header_deps": [],
            "inputs": [],
            "outputs": [],
            "outputs_data": [],
            "witnesses": []
        }
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
    "method": "estimate_cycles",
    "params": [
        {
            "version": "0x0",
            "cell_deps": [],
            "header_deps": [],
            "inputs": [],
            "outputs": [],
            "outputs_data": [],
            "witnesses": []
        }
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
    "method": "estimate_cycles",
    "params": [
        {
            "version": "0x0",
            "cell_deps": [],
            "header_deps": [],
            "inputs": [],
            "outputs": [],
            "outputs_data": [],
            "witnesses": []
        }
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
        "method": "estimate_cycles",
        "params": [
                {
                        "version": "0x0",
                        "cell_deps": [],
                        "header_deps": [],
                        "inputs": [],
                        "outputs": [],
                        "outputs_data": [],
                        "witnesses": []
                }
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
    "result": {
        "cycles": "0x3a980"
    }
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                            |
| ------------- | ------ | -------------------------------------- |
| result.cycles | Uint64 | Estimated CKB-VM cycles consumed (hex) |

## Use Cases

* Wallet pre-flight checks before broadcasting a transaction
* Computing transaction fees based on cycle count
* Detecting transactions that would exceed cycle limits

## Error Handling

| Status Code | Error Message                   | Cause                                                               |
| ----------- | ------------------------------- | ------------------------------------------------------------------- |
| 403         | Forbidden                       | Missing or invalid `<ACCESS-TOKEN>`                                 |
| -32602      | Invalid params                  | Request parameters are missing or malformed                         |
| -32601      | Method not found                | Method does not exist or is not enabled on this node                |
| -32603      | Internal error                  | Server-side error while processing the request                      |
| 429         | Too Many Requests               | Rate limit exceeded for your plan                                   |
| -32000      | Transaction failed verification | Scripts in the transaction would revert; estimation cannot complete |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('estimate_cycles', [{"version": "0x0", "cell_deps": [], "header_deps": [], "inputs": [], "outputs": [], "outputs_data": [], "witnesses": []}]);
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
    'method': 'estimate_cycles',
    'params': [{"version": "0x0", "cell_deps": [], "header_deps": [], "inputs": [], "outputs": [], "outputs_data": [], "witnesses": []}]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
