# get\_transactions monero

This non-JSON-RPC endpoint returns one or more transactions by hash. Optionally returns transaction data in fully decoded JSON form instead of as hex blobs.

## Parameters

| Parameter        | Type            | Required | Description                                                  |
| ---------------- | --------------- | -------- | ------------------------------------------------------------ |
| txs\_hashes      | array of string | Yes      | List of transaction hashes to look up                        |
| decode\_as\_json | boolean         | No       | If `true`, also return decoded JSON form of each transaction |
| prune            | boolean         | No       | If `true`, return pruned transaction data only               |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/get_transactions' \
--header 'Content-Type: application/json' \
--data-raw '{
    "txs_hashes": [
        "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
    ],
    "decode_as_json": false
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "txs_hashes": [
        "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
    ],
    "decode_as_json": false
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/get_transactions',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/get_transactions"

payload = json.dumps({
    "txs_hashes": [
        "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
    ],
    "decode_as_json": false
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
        "txs_hashes": [
                "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
        ],
        "decode_as_json": false
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/get_transactions")
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
    "status": "OK",
    "missed_tx": [],
    "txs": [
        {
            "as_hex": "020001020005b7\u2026",
            "block_height": 993442,
            "block_timestamp": 1457749396,
            "double_spend_seen": false,
            "in_pool": false,
            "output_indices": [
                198769,
                244799
            ],
            "tx_hash": "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
        }
    ],
    "txs_as_hex": [
        "020001020005b7\u2026"
    ],
    "untrusted": false
}
```
{% endcode %}

## Response Parameters

| Field                      | Type            | Description                                              |
| -------------------------- | --------------- | -------------------------------------------------------- |
| txs                        | array           | Array of transaction objects                             |
| txs\[].as\_hex             | string          | Hex-encoded transaction blob                             |
| txs\[].block\_height       | unsigned int    | Block in which the transaction was confirmed             |
| txs\[].block\_timestamp    | unsigned int    | Unix timestamp of the confirming block                   |
| txs\[].in\_pool            | boolean         | `true` if the transaction is still in the mempool        |
| txs\[].double\_spend\_seen | boolean         | `true` if a conflicting transaction has been seen        |
| missed\_tx                 | array of string | Hashes of any requested transactions that were not found |

## Use Cases

* Fetching transaction details for explorers and indexers
* Verifying transaction inclusion after broadcast
* Batch transaction lookups for analytics pipelines

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
// Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
// Use a plain HTTP client as shown in the Request Example above.
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/get_transactions', {"txs_hashes": ["d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"], "decode_as_json": false});
console.log(response.data);
```
{% endtab %}

{% tab title="monero-python" %}
```python
# Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
# Use a plain HTTP client as shown in the Request Example above.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/get_transactions', json={"txs_hashes": ["d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"], "decode_as_json": false})
print(response.json())
```
{% endtab %}
{% endtabs %}
