---
description: >-
  Example code for the decodepsbt jJSON RPC method. Сomplete guide on how to use
  decodepsbt JSON RPC in GetBlock Web3 documentation.
---

# decodepsbt - Bitcoin

This method decodes a PSBT (Partially Signed Bitcoin Transaction) and returns a detailed JSON object with all its components.

### Parameters

| Parameter | Type   | Required | Description             |
| --------- | ------ | -------- | ----------------------- |
| psbt      | string | Yes      | The PSBT base64 string. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "decodepsbt",
    "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez..."],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "decodepsbt",
    "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez..."],
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
    "method": "decodepsbt",
    "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez..."],
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
```ruby
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "decodepsbt",
            "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez..."],
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

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "tx": {
            "txid": "7e7b9b4c66e6af1e867adf7de48c8e5e5e1a90e60ad9c4c0ab2e8f8a...",
            "hash": "7e7b9b4c66e6af1e867adf7de48c8e5e5e1a90e60ad9c4c0ab2e8f8a...",
            "version": 2,
            "size": 117,
            "vsize": 117,
            "weight": 468,
            "locktime": 0,
            "vin": [
                {
                    "txid": "ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07",
                    "vout": 0,
                    "scriptSig": {
                        "asm": "",
                        "hex": ""
                    },
                    "sequence": 4294967295
                }
            ],
            "vout": [
                {
                    "value": 0.00100000,
                    "n": 0,
                    "scriptPubKey": {
                        "asm": "0 e8df018c7e326cc253f167d922d5b8e7cd25f1c9",
                        "hex": "0014e8df018c7e326cc253f167d922d5b8e7cd25f1c9",
                        "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
                        "type": "witness_v0_keyhash"
                    }
                }
            ]
        },
        "global_xpubs": [],
        "psbt_version": 0,
        "proprietary": [],
        "unknown": {},
        "inputs": [
            {
                "witness_utxo": {
                    "amount": 0.00200000,
                    "scriptPubKey": {
                        "asm": "0 a1b2c3d4e5f6...",
                        "hex": "0014a1b2c3d4e5f6...",
                        "type": "witness_v0_keyhash",
                        "address": "bc1q..."
                    }
                },
                "partial_signatures": {},
                "sighash": "ALL"
            }
        ],
        "outputs": [
            {}
        ],
        "fee": 0.00100000
    }
}
```

### Response Parameters

| Field                         | Type   | Description                                 |
| ----------------------------- | ------ | ------------------------------------------- |
| tx                            | object | The decoded unsigned transaction.           |
| tx.txid                       | string | The transaction id.                         |
| tx.version                    | number | The transaction version.                    |
| tx.locktime                   | number | The transaction locktime.                   |
| tx.vin                        | array  | Array of transaction inputs.                |
| tx.vout                       | array  | Array of transaction outputs.               |
| global\_xpubs                 | array  | Global extended public key information.     |
| psbt\_version                 | number | The PSBT version number.                    |
| inputs                        | array  | Array of PSBT input information.            |
| inputs\[].witness\_utxo       | object | The witness UTXO being spent.               |
| inputs\[].partial\_signatures | object | Partial signatures for the input.           |
| outputs                       | array  | Array of PSBT output information.           |
| fee                           | number | The transaction fee in BTC (if calculable). |

### Use Case

The `decodepsbt` method is essential for:

* Inspecting PSBT contents before signing
* Verifying transaction details in multi-sig workflows
* Debugging PSBT construction issues
* Displaying transaction information in wallet UIs
* Validating PSBTs received from other parties
* Auditing transaction metadata

### Error Handling

| Status Code | Error Message    | Cause                                    |
| ----------- | ---------------- | ---------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS-TOKEN.         |
| -22         | TX decode failed | The PSBT string is malformed or invalid. |

### Integration Notes

The `decodepsbt` method helps developers:

* Build transaction inspection interfaces
* Implement PSBT verification workflows
* Create transparent signing experiences
* Debug multi-party transaction construction
* Support hardware wallet verification displays
