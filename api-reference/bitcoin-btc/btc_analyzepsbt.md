---
description: >-
  Example code for the analyzepsbt JSON_RPC method. Сomplete guide on how to use
  analyzepsbt JSON-RPC in GetBlock Web3 documentation.
---

# analyzepsbt - Bitcoin

This method analyzes and provides information about the current status of a PSBT (Partially Signed Bitcoin Transaction) and its inputs.

### Parameters

| Parameter | Type   | Required | Description                |
| --------- | ------ | -------- | -------------------------- |
| psbt      | string | Yes      | A base64 string of a PSBT. |

### Request

{% tabs %}
{% tab title="Curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCES_TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "analyzepsbt",
    "params": [
        "cHNidP8BAEICAAAAATJIj43goxJCsLjdKM5Uly44S8Vp6cim1VW89Rx5fFPpAAAAAAD/////AQAAAAAAAAAABmoEAAECAwAAAAAAAAA="
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "analyzepsbt",
    "params": [
        "cHNidP8BAEICAAAAATJIj43goxJCsLjdKM5Uly44S8Vp6cim1VW89Rx5fFPpAAAAAAD/////AQAAAAAAAAAABmoEAAECAwAAAAAAAAA="
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS_TOKEN>',
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

url = "https://go.getblock.io/<ACCESS_TOKEN>"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "analyzepsbt",
    "params": [
        "cHNidP8BAEICAAAAATJIj43goxJCsLjdKM5Uly44S8Vp6cim1VW89Rx5fFPpAAAAAAD/////AQAAAAAAAAAABmoEAAECAwAAAAAAAAA="
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

{% tab title="Rust" %}
```ruby
use reqwest::header;

#[tokio::main]
async fn main() -> Result> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS_TOKEN>")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "analyzepsbt",
            "params": [
        "cHNidP8BAEICAAAAATJIj43goxJCsLjdKM5Uly44S8Vp6cim1VW89Rx5fFPpAAAAAAD/////AQAAAAAAAAAABmoEAAECAwAAAAAAAAA="
    ],
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
        "inputs": [
            {
                "has_utxo": true,
                "is_final": false,
                "next": "signer",
                "missing": {
                    "signatures": [
                        "02c7b8...8e1f"
                    ]
                }
            }
        ],
        "estimated_vsize": 147,
        "estimated_feerate": 0.00001000,
        "fee": 0.00000147,
        "next": "signer"
    }
}
```

### Response Parameters

| Field                        | Type    | Description                                                    |
| ---------------------------- | ------- | -------------------------------------------------------------- |
| inputs                       | array   | Array of objects with information about each input.            |
| inputs\[].has\_utxo          | boolean | Whether a UTXO is provided for this input.                     |
| inputs\[].is\_final          | boolean | Whether the input is finalized.                                |
| inputs\[].next               | string  | Role of the next person that this input needs to go to.        |
| inputs\[].missing            | object  | Object containing missing items needed for signing.            |
| inputs\[].missing.signatures | array   | Array of public keys whose corresponding signature is missing. |
| estimated\_vsize             | number  | Estimated virtual size of the final transaction in vbytes.     |
| estimated\_feerate           | number  | Estimated feerate of the final transaction in BTC/kvB.         |
| fee                          | number  | The transaction fee paid if all UTXOs are provided.            |
| next                         | string  | Role of the next person that this PSBT needs to go to.         |

### Use Case

The `analyzepsbt` method is essential for:

* Checking the completion status of a PSBT before signing
* Identifying missing signatures or other data
* Estimating transaction fees before finalizing
* Debugging PSBT construction issues
* Multi-signature workflow management
* Determining the next step in the PSBT workflow

### Error Handling

| Status Code | Error Message       | Cause                                           |
| ----------- | ------------------- | ----------------------------------------------- |
| 403         | RBAC: access denied | Missing or invalid ACCESS-TOKEN.                |
| -22         | Invalid params      | The PSBT string is malformed or invalid base64. |

### Integration Notes

The `analyzepsbt` method helps developers:

* Build multi-party signing workflows
* Validate PSBTs before broadcasting
* Calculate accurate fee estimates
* Track signing progress in hardware wallet integrations
* Implement PSBT-based payment protocols
