---
description: >-
  Example code for the deriveaddresses JSON RPCmethod. Сomplete guide on how to
  use deriveaddresses JSON RPC in GetBlock Web3 documentation.
---

# deriveaddresses - Bitcoin

This method derives one or more addresses corresponding to an output descriptor. Descriptors provide a way to express what scripts and keys are used in a wallet.

### Parameters

| Parameter  | Type         | Required | Description                                                                            |
| ---------- | ------------ | -------- | -------------------------------------------------------------------------------------- |
| descriptor | string       | Yes      | The output descriptor.                                                                 |
| range      | number/array | No       | If a ranged descriptor, the range to derive. Can be an integer or \[begin, end] array. |

### Request

{% tabs %}
{% tab title="Untitled" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "deriveaddresses",
    "params": ["wpkh([d34db33f/84h/0h/0h]xpub6ERApfZwUNrhLCkDtcHTcxd75RbzS1ed54G1LkBUHQVHQKqhMkhgbmJbZRkrgZw4koxb5JaHWkY4ALHY2grBGRjaDMzQLcgJvLJuZZvRcEL/0/*)#aq3rqr9j", [0, 2]],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "deriveaddresses",
    "params": ["wpkh([d34db33f/84h/0h/0h]xpub6ERApfZwUNrhLCkDtcHTcxd75RbzS1ed54G1LkBUHQVHQKqhMkhgbmJbZRkrgZw4koxb5JaHWkY4ALHY2grBGRjaDMzQLcgJvLJuZZvRcEL/0/*)#aq3rqr9j", [0, 2]],
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
    "method": "deriveaddresses",
    "params": ["wpkh([d34db33f/84h/0h/0h]xpub6ERApfZwUNrhLCkDtcHTcxd75RbzS1ed54G1LkBUHQVHQKqhMkhgbmJbZRkrgZw4koxb5JaHWkY4ALHY2grBGRjaDMzQLcgJvLJuZZvRcEL/0/*)#aq3rqr9j", [0, 2]],
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
            "method": "deriveaddresses",
            "params": ["wpkh([d34db33f/84h/0h/0h]xpub6ERApfZwUNrhLCkDtcHTcxd75RbzS1ed54G1LkBUHQVHQKqhMkhgbmJbZRkrgZw4koxb5JaHWkY4ALHY2grBGRjaDMzQLcgJvLJuZZvRcEL/0/*)#aq3rqr9j", [0, 2]],
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
    "result": [
        "bc1qcr8te4kr609gcawutmrza0j4xv80jy8z306fyu",
        "bc1qnjg0jd8228aq7uj72e4k5l95vjzp9ymcflfz5t",
        "bc1qp59yckz4ae5c4efgw2s5wfyvrz0ala7rgvuzcy"
    ]
}
```

### Response Parameters

| Field  | Type  | Description                         |
| ------ | ----- | ----------------------------------- |
| result | array | Array of derived Bitcoin addresses. |

### Use Case

The `deriveaddresses` method is essential for:

* Generating addresses from HD wallet descriptors
* Verifying address derivation paths
* Building watch-only wallets
* Implementing address gap limit scanning
* Supporting BIP32/BIP44/BIP84 address derivation
* Creating address generation tools

### Error Handling

| Status Code | Error Message      | Cause                                       |
| ----------- | ------------------ | ------------------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN.            |
| -5          | Invalid descriptor | The descriptor syntax is incorrect.         |
| -8          | Range not provided | Ranged descriptor requires range parameter. |

### Integration Notes

The `deriveaddresses` method helps developers:

* Build HD wallet implementations
* Generate receiving addresses
* Implement address discovery algorithms
* Support multiple address types (legacy, SegWit, Taproot)
* Create backup verification tools
