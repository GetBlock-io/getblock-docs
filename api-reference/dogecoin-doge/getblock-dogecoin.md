---
description: >-
  Example code for the getblock JSON-RPC method. Complete guide on how to use
  getblock JSON-RPC in GetBlock Web3 documentation.
---

# getblock - Dogecoin

This method returns block data for a given block hash.

## Parameters

| Parameter | Type   | Required | Description                        |
| --------- | ------ | -------- | ---------------------------------- |
| blockhash | string | Yes      | The hash of the block to retrieve. |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblock",
    "params": ["24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getblock",
    "params": ["24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa"],
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
{% endtab %}

{% tab title="Request" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getblock",
    "params": ["24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust" %}
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
            "method": "getblock",
            "params": ["24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa"],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

{% code overflow="wrap" %}
```json
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "auxpow": {
            "chainindex": 0,
            "chainmerklebranch": [],
            "index": 0,
            "merklebranch": [...],
            "parentblock": "000000207a9ac39b...",
            "tx": {
                "blockhash": "e60fd6dce484786fa8a1caefbf76d798c0a8aa64becb9897cf41b7aa4a251233",
                "hash": "10c03013c4feadd7d2e0d948240771dbbc220152d12c3ab7628a40a7f827f658",
                "hex": "01000000010000000000000000...",
                "locktime": 0,
                "size": 214,
                "txid": "10c03013c4feadd7d2e0d948240771dbbc220152d12c3ab7628a40a7f827f658",
                "version": 1,
                "vin": [...],
                "vout": [...],
                "vsize": 214
            }
        },
        "bits": "1a04c252",
        "chainwork": "0000000000000000000000000000000000000000000005c2f27be4a1f414f1c0",
        "confirmations": 1389,
        "difficulty": 3525264.838757254,
        "hash": "24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa",
        "height": 3904788,
        "mediantime": 1632136417,
        "merkleroot": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7",
        "nextblockhash": "b53179ba74966931249faf735fc49af1b1ad686f8b12bb3ae6dcd7a35243e358",
        "nonce": 0,
        "previousblockhash": "f9e8b36d03890feae429b2810c086683dca8eec65c5b7e0ca9fca7a4540fd1f4",
        "size": 828,
        "strippedsize": 828,
        "time": 1632136777,
        "tx": [
            "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7"
        ],
        "version": 6422788,
        "versionHex": "00620104",
        "weight": 3312
    }
}
```
{% endcode %}

## Response Parameters

| Field             | Type   | Description                                       |
| ----------------- | ------ | ------------------------------------------------- |
| hash              | string | The block hash.                                   |
| confirmations     | number | Number of confirmations.                          |
| height            | number | Block height.                                     |
| version           | number | Block version.                                    |
| versionHex        | string | Block version in hex.                             |
| merkleroot        | string | Merkle root of transactions.                      |
| time              | number | Block timestamp.                                  |
| mediantime        | number | Median time of previous 11 blocks.                |
| nonce             | number | Nonce value.                                      |
| bits              | string | Compact difficulty target.                        |
| difficulty        | number | Mining difficulty.                                |
| chainwork         | string | Total work in chain.                              |
| previousblockhash | string | Previous block hash.                              |
| nextblockhash     | string | Next block hash.                                  |
| auxpow            | object | Auxiliary proof of work data (for merged mining). |
| tx                | array  | Array of transaction IDs.                         |
| size              | number | Block size in bytes.                              |
| weight            | number | Block weight.                                     |

## Use Case

The `getblock` method is essential for:

* Block explorer functionality
* Transaction verification
* Chain analysis and statistics
* Historical data retrieval
* Mining pool block tracking

## Error Handling

| Error Code | Message         | Cause                            |
| ---------- | --------------- | -------------------------------- |
| -5         | Block not found | Invalid block hash provided.     |
| 403        | Forbidden       | Missing or invalid ACCESS-TOKEN. |
