---
description: >-
  Example code for the avm.getTx JSON-RPC method. Complete guide on how to use
  avm.getTx JSON-RPC in GetBlock Web3 documentation.
---

# avm.getTx - AVAX

Returns an X-Chain transaction by its ID. Useful for inspecting any past transfer or atomic transaction.

## Parameters

| Parameter | Type   | Required | Description                                                  |
| --------- | ------ | -------- | ------------------------------------------------------------ |
| txID      | string | Yes      | Transaction ID (CB58)                                        |
| encoding  | string | No       | Response encoding — `"hex"` (default), `"cb58"`, or `"json"` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "avm.getTx",
    "params": {
        "txID": "2oyUzwwCLpYn6MyKyNaJe2FcCgXqnmNENyiCNKocfsWcjvmard",
        "encoding": "json"
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
    "method": "avm.getTx",
    "params": {
        "txID": "2oyUzwwCLpYn6MyKyNaJe2FcCgXqnmNENyiCNKocfsWcjvmard",
        "encoding": "json"
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
    "method": "avm.getTx",
    "params": {
        "txID": "2oyUzwwCLpYn6MyKyNaJe2FcCgXqnmNENyiCNKocfsWcjvmard",
        "encoding": "json"
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
        "method": "avm.getTx",
        "params": {
            "txID": "2oyUzwwCLpYn6MyKyNaJe2FcCgXqnmNENyiCNKocfsWcjvmard",
            "encoding": "json"
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
    "result": {
        "tx": {
            "unsignedTx": {
                "networkID": 1,
                "blockchainID": "2oYMBNV4eNHyqk2fjjV5nVQLDbtmNJzq5s3qs3Lo6ftnC6FByM",
                "outputs": [
                    {
                        "assetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
                        "fxID": "spdxUxVJQbX85MGxMHbKw1sHxMnSqJ3QBzDyDYEP3h6TLuxqQ",
                        "output": {
                            "addresses": [
                                "X-avax1kgxpsws29gfzsm50ge4yucp3fjpzznklcckzag"
                            ],
                            "amount": 644347000,
                            "locktime": 0,
                            "threshold": 1
                        }
                    },
                    {
                        "assetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
                        "fxID": "spdxUxVJQbX85MGxMHbKw1sHxMnSqJ3QBzDyDYEP3h6TLuxqQ",
                        "output": {
                            "addresses": [
                                "X-avax1y9gqplplrrmpqdqgp0k6ketpufny2k9vxnlfry"
                            ],
                            "amount": 216823360653000,
                            "locktime": 0,
                            "threshold": 1
                        }
                    }
                ],
                "inputs": [
                    {
                        "txID": "nn1fVC6vzYdP1U1kc8scrX6ETEfBSyPA1vDhLFEYXLsyF6px5",
                        "outputIndex": 1,
                        "assetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
                        "fxID": "spdxUxVJQbX85MGxMHbKw1sHxMnSqJ3QBzDyDYEP3h6TLuxqQ",
                        "input": {
                            "amount": 216824006000000,
                            "signatureIndices": [
                                0
                            ]
                        }
                    }
                ],
                "memo": "0x"
            },
            "credentials": [
                {
                    "fxID": "spdxUxVJQbX85MGxMHbKw1sHxMnSqJ3QBzDyDYEP3h6TLuxqQ",
                    "credential": {
                        "signatures": [
                            "0xcdee8b67f426e4d514e598ea32aebbf479724b0941bbaae983eb360178d6f6b66b8da70ab0dcd04e4e7bbca912357145194d54c1e593c3b06cd597a965f13b4101"
                        ]
                    }
                }
            ],
            "id": "2oyUzwwCLpYn6MyKyNaJe2FcCgXqnmNENyiCNKocfsWcjvmard"
        },
        "encoding": "json"
    },
    "id": "getblock.io"
}

```
{% endcode %}

## Response Parameters

| Field           | Type             | Description                                                               |
| --------------- | ---------------- | ------------------------------------------------------------------------- |
| result.tx       | object \| string | Transaction body — object when `encoding=json`, hex/cb58 string otherwise |
| result.encoding | string           | Encoding used                                                             |

## Use Cases

* Inspecting historical X-Chain transactions
* Building X-Chain explorers
* Audit trails and forensics on native asset transfers

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
    method: 'avm.getTx',
    params: {
        txID: "2kxwWpHvZPhMsJcSTmM7a3Da7sExB8pPyF7t4cr2NSwnYqNHni",
        encoding: "json"
    }
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
    'method': 'avm.getTx',
    'params': {
        "txID": "2kxwWpHvZPhMsJcSTmM7a3Da7sExB8pPyF7t4cr2NSwnYqNHni",
        "encoding": "json"
    }
}
response = requests.post(url, json=payload)
print(response.json())
```
{% endtab %}
{% endtabs %}
