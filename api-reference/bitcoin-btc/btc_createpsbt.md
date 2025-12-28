---
description: >-
  Example code for the createpsbt JSON RPC method. Сomplete guide on how to use
  createpsbt JSON RPC in GetBlock Web3 documentation.
---

# createpsbt - Bitcoin

This method creates a PSBT (Partially Signed Bitcoin Transaction) with the given inputs and outputs. The transaction is unsigned and can be signed later using wallets or signing devices.

### Parameters

| Parameter          | Type    | Required | Description                                                   |
| ------------------ | ------- | -------- | ------------------------------------------------------------- |
| inputs             | array   | Yes      | Array of input objects with txid and vout.                    |
| inputs\[].txid     | string  | Yes      | The transaction id of the UTXO to spend.                      |
| inputs\[].vout     | number  | Yes      | The output index of the UTXO to spend.                        |
| inputs\[].sequence | number  | No       | The sequence number (default: depends on locktime).           |
| outputs            | array   | Yes      | Array of output objects mapping addresses to amounts.         |
| locktime           | number  | No       | Raw locktime (default: 0).                                    |
| replaceable        | boolean | No       | Marks this transaction as BIP125-replaceable (default: true). |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "createpsbt",
    "params": [
        [{"txid": "ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07", "vout": 0}],
        [{"bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq": 0.001}]
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
    "method": "createpsbt",
    "params": [
        [{"txid": "ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07", "vout": 0}],
        [{"bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq": 0.001}]
    ],
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
    "method": "createpsbt",
    "params": [
        [{"txid": "ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07", "vout": 0}],
        [{"bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq": 0.001}]
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
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "createpsbt",
            "params": [
                [{"txid": "ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07", "vout": 0}],
                [{"bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq": 0.001}]
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
    "result": "cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez..."
}
```

### Response Parameters

| Field  | Type   | Description                                   |
| ------ | ------ | --------------------------------------------- |
| result | string | The resulting unsigned PSBT in base64 format. |

### Use Case

The `createpsbt` method is essential for:

* Building unsigned transactions for hardware wallet signing
* Creating multi-signature transaction proposals
* Constructing complex transactions with multiple inputs/outputs
* Implementing cold storage spending workflows
* Building transaction batching systems
* Creating RBF-enabled transactions

### Error Handling

| Status Code | Error Message           | Cause                                       |
| ----------- | ----------------------- | ------------------------------------------- |
| 403         | Forbidden               | Missing or invalid ACCESS-TOKEN.            |
| -32602      | Invalid params          | Missing or malformed inputs/outputs arrays. |
| -8          | Invalid parameter       | Invalid address or amount in outputs.       |
| -5          | Invalid Bitcoin address | One of the output addresses is invalid.     |

### Integration Notes

The `createpsbt` method helps developers:

* Build wallet applications with hardware wallet support
* Implement multi-party transaction construction
* Create offline transaction generation systems
* Support air-gapped signing workflows
* Enable secure cold storage spending
