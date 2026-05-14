---
description: >-
  Example code for the getextendedaddressinformation JSON-RPC method. Сomplete
  guide on how to use getextendedaddressinformation JSON-RPC in GetBlock.io Web3
  documentation.
---

# getextendedaddressinformation - TON

This method returns extended information about a TON address, including parsed details for known contract types. It is based on tonlib's `getAccountState` function and provides more structure than `getAddressInformation`. For detecting wallets specifically, prefer `getWalletInformation`.

## Parameters

| Parameter | Type    | Required | Description                                                            |
| --------- | ------- | -------- | ---------------------------------------------------------------------- |
| address   | string  | Yes      | Identifier of the target TON account in any form                       |
| seqno     | integer | No       | Masterchain seqno at which to read state. Defaults to the latest block |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/getExtendedAddressInformation?address=EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2' \
--header 'Content-Type: application/json'
```

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getExtendedAddressInformation",
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
    "method": "getExtendedAddressInformation",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
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

url = "https://go.getblock.io/<ACCESS-TOKEN>"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getExtendedAddressInformation",
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
        "method": "getExtendedAddressInformation",
        "params": {
                "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>")
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
        "@type": "fullAccountState",
        "address": {
            "@type": "accountAddress",
            "account_address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
        },
        "balance": "1234567890",
        "last_transaction_id": {
            "@type": "internal.transactionId",
            "lt": "47652103000003",
            "hash": "AbCdEfGhIjKlMnOpQrStUvWxYz0123456789+/="
        },
        "block_id": {
            "@type": "ton.blockIdExt",
            "workchain": -1,
            "shard": "-9223372036854775808",
            "seqno": 45792554
        },
        "sync_utime": 1737869732,
        "account_state": {
            "@type": "wallet.v4.accountState",
            "wallet_id": "698983191",
            "seqno": 42
        },
        "revision": 2
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field                            | Type    | Description                                                    |
| -------------------------------- | ------- | -------------------------------------------------------------- |
| ok                               | boolean | Whether the request succeeded                                  |
| result.address.account\_address  | string  | User-friendly representation of the account address            |
| result.balance                   | string  | Balance in nanotons                                            |
| result.account\_state.@type      | string  | Parsed account state type (e.g. `wallet.v4.accountState`)      |
| result.account\_state.seqno      | integer | Wallet seqno for wallet contracts (used as nonce when sending) |
| result.account\_state.wallet\_id | string  | Wallet subwallet identifier where applicable                   |
| result.revision                  | integer | Contract revision number for typed contracts                   |
| result.sync\_utime               | integer | Unix timestamp at which state was synchronised                 |

## Use Cases

* Detecting wallet type and version (v3, v4, w5, etc.)
* Retrieving wallet seqno before constructing a transfer
* Building explorers that show parsed contract state
* Validating known contract types before interaction

## Error Handling

| Status Code | Error Message       | Cause                                       |
| ----------- | ------------------- | ------------------------------------------- |
| 403         | Forbidden           | Missing or invalid `<ACCESS-TOKEN>`         |
| 422         | Validation Error    | Request parameters are missing or malformed |
| 429         | Too Many Requests   | Rate limit exceeded for your plan           |
| 504         | Lite Server Timeout | Upstream TON liteserver timed out           |

## SDK Integration

{% tabs %}
{% tab title="@ton/ton (JavaScript)" %}
```javascript
import { TonClient } from '@ton/ton';

const client = new TonClient({
    endpoint: 'https://go.getblock.io/<ACCESS-TOKEN>'
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
