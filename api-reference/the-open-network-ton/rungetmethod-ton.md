---
description: >-
  Example code for the rungetmethod JSON-RPC method. Сomplete guide on how to
  use rungetmethod JSON-RPC in GetBlock.io Web3 documentation.
---

# rungetmethod - TON

This method invokes a read-only get-method on a TON smart contract and returns the TVM stack result. Get-methods do not change contract state and incur no fees — they are the standard way to query contract-defined data such as Jetton wallet balances or NFT metadata.

## Parameters

| Parameter | Type              | Required | Description                                                   |
| --------- | ----------------- | -------- | ------------------------------------------------------------- |
| address   | string            | Yes      | Address of the smart contract                                 |
| method    | string or integer | Yes      | Method name (e.g. `get_wallet_data`) or its numeric method-id |
| stack     | array             | Yes      | Input arguments as TVM stack entries, e.g. `[["num", "42"]]`  |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "runGetMethod",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "method": "get_wallet_data",
        "stack": []
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
    "method": "runGetMethod",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "method": "get_wallet_data",
        "stack": []
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>',
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
    "method": "runGetMethod",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "method": "get_wallet_data",
        "stack": []
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
        "method": "runGetMethod",
        "params": {
                "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
                "method": "get_wallet_data",
                "stack": []
        },
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
    "ok": true,
    "result": {
        "@type": "smc.runResult",
        "gas_used": 1500,
        "stack": [
            [
                "num",
                "0x174876E800"
            ]
        ],
        "exit_code": 0,
        "block_id": {
            "@type": "ton.blockIdExt",
            "workchain": -1,
            "shard": "-9223372036854775808",
            "seqno": 45792554
        }
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field             | Type    | Description                                                             |
| ----------------- | ------- | ----------------------------------------------------------------------- |
| result.gas\_used  | integer | Gas units consumed by the get-method                                    |
| result.stack      | array   | TVM stack entries returned by the method, each as `[type, value]`       |
| result.exit\_code | integer | TVM exit code (`0` and `1` indicate success; anything else is an error) |
| result.block\_id  | object  | Block ID at which the method was executed                               |

## Use Cases

* Reading Jetton wallet balances via `get_wallet_data`
* Reading NFT item content via `get_nft_data`
* Reading arbitrary on-chain state from custom contracts
* DEX pool pricing and reserve lookups

## Error Handling

| Status Code | Error Message       | Cause                                       |
| ----------- | ------------------- | ------------------------------------------- |
| 403         | Forbidden           | Missing or invalid `<ACCESS-TOKEN>`         |
| 422         | Validation Error    | Request parameters are missing or malformed |
| 429         | Too Many Requests   | Rate limit exceeded for your plan           |
| 504         | Lite Server Timeout | Upstream TON liteserver timed out           |
| 422         | Validation Error    | Method or stack types are malformed         |
| 500         | TVM execution error | Get-method returned a non-zero exit code    |

## SDK Integration

{% tabs %}
{% tab title="@ton/ton (JavaScript)" %}
```javascript
import { TonClient } from '@ton/ton';

const client = new TonClient({
    endpoint: 'https://go.getblock.io/<ACCESS-TOKEN>/'
});

// The @ton/ton client wraps the same JSON-RPC methods.
// For low-level access, use client.callGetMethod or client.provider.
```
{% endtab %}

{% tab title="pytoniq (Python)" %}
```python
from pytoniq import LiteClient
# For HTTP API access, use ton-http-client or call the JSON-RPC
# endpoint directly with requests as shown in the Request Example.
```
{% endtab %}
{% endtabs %}
