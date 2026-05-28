---
description: >-
  Example code for the avm.getUTXOs JSON-RPC method. Complete guide on how to
  use avm.getUTXOs JSON-RPC in GetBlock Web3 documentation.
---

# avm.getUTXOs - AVAX

Returns the UTXOs that reference one or more X-Chain addresses. UTXOs are returned encoded — wallets parse them to construct new transactions.

## Parameters

| Parameter  | Type            | Required | Description                            |
| ---------- | --------------- | -------- | -------------------------------------- |
| addresses  | array of string | Yes      | Array of X-Chain Bech32 addresses      |
| limit      | integer         | No       | Maximum number of UTXOs to return      |
| startIndex | object          | No       | Pagination cursor from a previous call |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "avm.getUTXOs",
    "params": {
        "addresses": [
            "X-avax1fw57u4tp7xzx0k6ufn7tj9caua59mt9gqcvy7m"
        ],
        "limit": 100
    },
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "avm.getUTXOs",
    "params": {
        "addresses": [
            "X-avax1fw57u4tp7xzx0k6ufn7tj9caua59mt9gqcvy7m"
        ],
        "limit": 100
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "avm.getUTXOs",
    "params": {
        "addresses": [
            "X-avax1fw57u4tp7xzx0k6ufn7tj9caua59mt9gqcvy7m"
        ],
        "limit": 100
    },
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
        "method": "avm.getUTXOs",
        "params": {
            "addresses": [
                "X-avax1fw57u4tp7xzx0k6ufn7tj9caua59mt9gqcvy7m"
            ],
            "limit": 100
        },
        "id": "getblock.io"
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X")
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
        "numFetched": "1",
        "utxos": [
            "0x000021e67317cbc4be2aeb00677ad6462778a8f52274b9d605df2591b23027a87dff..."
        ],
        "endIndex": {
            "address": "X-avax1fw57u4tp7xzx0k6ufn7tj9caua59mt9gqcvy7m",
            "utxo": "11111111111111111111111111111111LpoYY"
        },
        "encoding": "hex"
    }
}
```
{% endcode %}

## Response Parameters

| Field             | Type            | Description                               |
| ----------------- | --------------- | ----------------------------------------- |
| result.numFetched | string          | Number of UTXOs returned (decimal string) |
| result.utxos      | array of string | Encoded UTXOs                             |
| result.endIndex   | object          | Pagination cursor for the next call       |
| result.encoding   | string          | Encoding used                             |

## Use Cases

* Constructing new X-Chain transactions
* Wallet sync flows for X-Chain accounts
* UTXO-set indexing

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="AvalancheJS / Axios" %}
```javascript
// AvalancheJS provides typed clients for P-Chain and X-Chain.
// X-Chain operations are exposed through `avm` in AvalancheJS.
// For raw JSON-RPC access, use a generic HTTP client as in the Request Example.
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'avm.getUTXOs',
    params: {"addresses": ["X-avax1fw57u4tp7xzx0k6ufn7tj9caua59mt9gqcvy7m"], "limit": 100}
});
console.log(response.data);
```
{% endtab %}

{% tab title="Python" %}
```python
# AvalancheJS is JavaScript-only. For Python, use a generic JSON-RPC client.
import requests
import json

url = 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X'
payload = {
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'avm.getUTXOs',
    'params': {"addresses": ["X-avax1fw57u4tp7xzx0k6ufn7tj9caua59mt9gqcvy7m"], "limit": 100}
}
response = requests.post(url, json=payload)
print(response.json())
```
{% endtab %}
{% endtabs %}
