---
description: >-
  Example code for the get_outs JSON-RPC method. Complete guide on how to use
  get_outs JSON-RPC in GetBlock Web3 documentation.
---

# get\_outs - Monero

This non-JSON-RPC endpoint returns transaction outputs by their global output index. Wallets use this to fetch decoys when constructing ring signatures.

## Parameters

| Parameter | Type    | Required | Description                                           |
| --------- | ------- | -------- | ----------------------------------------------------- |
| outputs   | array   | Yes      | Array of `{amount, index}` pairs to look up           |
| get\_txid | boolean | No       | If `true`, also return the originating transaction ID |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/get_outs' \
--header 'Content-Type: application/json' \
--data-raw '{
    "outputs": [
        {
            "amount": 1000000000000,
            "index": 0
        }
    ],
    "get_txid": true
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "outputs": [
        {
            "amount": 1000000000000,
            "index": 0
        }
    ],
    "get_txid": true
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/get_outs',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/get_outs"

payload = json.dumps({
    "outputs": [
        {
            "amount": 1000000000000,
            "index": 0
        }
    ],
    "get_txid": true
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
        "outputs": [
                {
                        "amount": 1000000000000,
                        "index": 0
                }
        ],
        "get_txid": true
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/get_outs")
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
    "outs": [
        {
            "height": 51941,
            "key": "08980d939ec297dd597119f498eb69e2ed009b1c2ee5fd3c2b97f12d3b30b372",
            "mask": "a6ccfca29435f6d27e9706b48a37a6b3a8ec9aaee1f9ace5e9c0bb9e3a59c91d",
            "txid": "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408",
            "unlocked": true
        }
    ],
    "status": "OK",
    "untrusted": false
}
```
{% endcode %}

## Response Parameters

| Field            | Type         | Description                                                          |
| ---------------- | ------------ | -------------------------------------------------------------------- |
| outs             | array        | Array of output records                                              |
| outs\[].key      | string       | Output public key                                                    |
| outs\[].mask     | string       | Commitment mask for RingCT outputs                                   |
| outs\[].txid     | string       | Hash of the transaction that created the output (if `get_txid=true`) |
| outs\[].height   | unsigned int | Block height where the output was created                            |
| outs\[].unlocked | boolean      | `true` if the output is currently spendable                          |

## Use Cases

* Wallet decoy selection for transaction construction
* Output-tracking analytics
* Chain audits and research on output unlock schedules

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
// Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
// Use a plain HTTP client as shown in the Request Example above.
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/get_outs', {"outputs": [{"amount": 1000000000000, "index": 0}], "get_txid": true});
console.log(response.data);
```
{% endtab %}

{% tab title="monero-python" %}
```python
// Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
// Use a plain HTTP client as shown in the Request Example above.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/get_outs', json={"outputs": [{"amount": 1000000000000, "index": 0}], "get_txid": true})
print(response.json())
```
{% endtab %}
{% endtabs %}
