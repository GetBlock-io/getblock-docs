---
description: >-
  Example code for the gettokendata JSON-RPC method. Сomplete guide on how to
  use gettokendata JSON-RPC in GetBlock.io Web3 documentation.
---

# gettokendata - TON

This method returns Jetton (TIP-3) or NFT (TIP-4) metadata read directly from the token's smart contract. It detects the standard automatically and returns either Jetton master / wallet data or NFT collection / item data.

## Parameters

| Parameter | Type   | Required | Description                                                              |
| --------- | ------ | -------- | ------------------------------------------------------------------------ |
| address   | string | Yes      | Address of the Jetton master, Jetton wallet, NFT collection, or NFT item |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/getTokenData?address=EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2' \
--header 'Content-Type: application/json'
```

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getTokenData",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
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
    "method": "getTokenData",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
    },
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
    "method": "getTokenData",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
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
        "method": "getTokenData",
        "params": {
                "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
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
        "total_supply": "1000000000000000",
        "mintable": false,
        "admin_address": "EQB\u2026",
        "jetton_content": {
            "type": "onchain",
            "data": {
                "name": "Example Jetton",
                "symbol": "EJT",
                "decimals": "9",
                "description": "An example Jetton on TON",
                "image": "https://example.com/logo.png"
            }
        },
        "jetton_wallet_code": "te6cckECEgEAAlMAART/APSkE/S88sgLAQ\u2026",
        "contract_type": "jetton_master"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field                       | Type    | Description                                                                            |
| --------------------------- | ------- | -------------------------------------------------------------------------------------- |
| result.contract\_type       | string  | Detected token type: `jetton_master`, `jetton_wallet`, `nft_collection`, or `nft_item` |
| result.total\_supply        | string  | Total supply for Jetton masters (in the token's smallest unit)                         |
| result.mintable             | boolean | Whether the Jetton supply can be increased                                             |
| result.admin\_address       | string  | Admin address of the Jetton master, if any                                             |
| result.jetton\_content      | object  | Token metadata, either `onchain` or `offchain` with a URI                              |
| result.jetton\_wallet\_code | string  | BOC of the wallet contract code (Jetton master only)                                   |

## Use Cases

* Reading Jetton symbol, decimals, and supply for wallet UX
* Fetching NFT collection or item metadata
* Building token explorers and DEX listings
* Validating a token contract is well-formed before listing

## Error Handling

| Status Code | Error Message                         | Cause                                         |
| ----------- | ------------------------------------- | --------------------------------------------- |
| 403         | Forbidden                             | Missing or invalid `<ACCESS-TOKEN>`           |
| 422         | Validation Error                      | Request parameters are missing or malformed   |
| 429         | Too Many Requests                     | Rate limit exceeded for your plan             |
| 504         | Lite Server Timeout                   | Upstream TON liteserver timed out             |
| 409         | Smart contract is not a Jetton or NFT | The address does not implement TIP-3 or TIP-4 |

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
