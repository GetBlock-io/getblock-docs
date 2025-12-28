---
description: >-
  Example code for the decodescript JSON RPC method. Сomplete guide on how to
  use decodescript JSON RPC in GetBlock.io Web3 documentation.
---

# decodescript - Bitcoin

## decodescript - Bitcoin

Example code for the decodescript JSON-RPC method. Complete guide on how to use decodescript JSON-RPC in GetBlock Web3 documentation.

This method decodes a hex-encoded script and returns information about the script type, required signatures, and addresses.

### Parameters

| Parameter | Type   | Required | Description                    |
| --------- | ------ | -------- | ------------------------------ |
| hexstring | string | Yes      | The hex-encoded script string. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "decodescript",
    "params": ["76a91489abcdefabbaabbaabbaabbaabbaabbaabbaabba88ac"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "decodescript",
    "params": ["76a91489abcdefabbaabbaabbaabbaabbaabbaabbaabba88ac"],
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
    "method": "decodescript",
    "params": ["76a91489abcdefabbaabbaabbaabbaabbaabbaabbaabba88ac"],
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
            "method": "decodescript",
            "params": ["76a91489abcdefabbaabbaabbaabbaabbaabbaabbaabba88ac"],
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
        "asm": "OP_DUP OP_HASH160 89abcdefabbaabbaabbaabbaabbaabbaabbaabba OP_EQUALVERIFY OP_CHECKSIG",
        "type": "pubkeyhash",
        "address": "1DYwPTpZuLjY2qApmJdHaSAuWRvEF5skCN",
        "reqSigs": 1,
        "p2sh": "3F6i6kwkevjR7AsAd4te2YB2zZyASEm1HM",
        "segwit": {
            "asm": "0 89abcdefabbaabbaabbaabbaabbaabbaabbaabba",
            "hex": "001489abcdefabbaabbaabbaabbaabbaabbaabbaabba",
            "address": "bc1q3x4umaa4w4twa9wa4wa9wa4wa9wa4waav4kwz9",
            "type": "witness_v0_keyhash",
            "p2sh-segwit": "3Kp6VqwVFECmvPxjvs2tn3dT2JZQmZVrRq"
        }
    }
}
```

### Response Parameters

| Field              | Type   | Description                                               |
| ------------------ | ------ | --------------------------------------------------------- |
| asm                | string | The script in human-readable assembly format.             |
| type               | string | The output type (e.g., pubkeyhash, scripthash, multisig). |
| address            | string | The Bitcoin address for this script (if applicable).      |
| reqSigs            | number | The required number of signatures.                        |
| addresses          | array  | Array of Bitcoin addresses (for multisig scripts).        |
| p2sh               | string | The P2SH address wrapping this script.                    |
| segwit             | object | SegWit-specific information.                              |
| segwit.asm         | string | The SegWit script in assembly format.                     |
| segwit.hex         | string | The SegWit script in hex.                                 |
| segwit.address     | string | The native SegWit address.                                |
| segwit.type        | string | The SegWit script type.                                   |
| segwit.p2sh-segwit | string | The P2SH-wrapped SegWit address.                          |

### Use Case

The `decodescript` method is essential for:

* Analyzing Bitcoin script functionality
* Understanding multisig requirements
* Converting scripts to addresses
* Debugging custom scripts
* Verifying script compatibility
* Building script analysis tools

### Error Handling

| Status Code | Error Message  | Cause                            |
| ----------- | -------------- | -------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN. |
| -22         | Invalid script | The script hex is malformed.     |

### Integration Notes

The `decodescript` method helps developers:

* Build script debugging tools
* Implement address derivation from scripts
* Analyze multisig configurations
* Support various address format conversions
* Create script visualization interfaces
