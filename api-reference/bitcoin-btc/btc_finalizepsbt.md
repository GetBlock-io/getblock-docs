---
description: >-
  Example code for the finalizepsbt JSON RPC method. Сomplete guide on how to
  use finalizepsbt JSON RPC in GetBlock Web3 documentation.
---

# finalizepsbt - Bitcoin

This method finalizes the inputs of a PSBT. If the transaction is fully signed, it will produce a network-serialized transaction that can be broadcast.

### Parameters

| Parameter | Type    | Required | Description                                                                                                  |
| --------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------ |
| psbt      | string  | Yes      | A base64 string of a PSBT.                                                                                   |
| extract   | boolean | No       | If true and the transaction is complete, extract and return the complete transaction in hex (default: true). |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "finalizepsbt",
    "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez...", true],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "finalizepsbt",
    "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez...", true],
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
    "method": "finalizepsbt",
    "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez...", True],
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
            "method": "finalizepsbt",
            "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez...", true],
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
        "psbt": "cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez...",
        "hex": "0200000001074a0bfaf4462cef0a5665b89fd7fd5e4f8536630cde6824d09b20400b2f65ee000000006a47304402207...",
        "complete": true
    }
}
```

### Response Parameters

| Field    | Type    | Description                                                         |
| -------- | ------- | ------------------------------------------------------------------- |
| psbt     | string  | The finalized PSBT in base64 format (if not extracted).             |
| hex      | string  | The hex-encoded network transaction (if complete and extract=true). |
| complete | boolean | Whether the transaction has a complete set of signatures.           |

### Use Case

The `finalizepsbt` method is essential for:

* Completing signed PSBTs for broadcast
* Converting PSBTs to raw transactions
* Verifying PSBT signing completion
* Building transaction finalization workflows
* Preparing transactions for network submission
* Supporting hardware wallet signing flows

### Error Handling

| Status Code | Error Message    | Cause                            |
| ----------- | ---------------- | -------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS-TOKEN. |
| -22         | TX decode failed | The PSBT string is malformed.    |

### Integration Notes

The `finalizepsbt` method helps developers:

* Complete multi-party signing workflows
* Prepare transactions for broadcast
* Validate signing completion
* Support hardware wallet integrations
* Build transaction submission pipelines
