---
description: >-
  Example code for the combinerawtransaction JSON RPC method. Сomplete guide on
  how to use combinerawtransaction JSON RPC in GetBlock Web3 documentation.
---

# combinerawtransaction - Bitcoin

This method combines multiple partially signed raw transactions into one transaction. The combined transaction may be another partially signed transaction or a fully signed transaction.

### Parameters

| Parameter | Type  | Required | Description                                               |
| --------- | ----- | -------- | --------------------------------------------------------- |
| txs       | array | Yes      | An array of hex strings of partially signed transactions. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "combinerawtransaction",
    "params": [["0200000001aad73931018bd25f84ae4...", "0200000001aad73931018bd25f84ae4..."]],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "combinerawtransaction",
    "params": [["0200000001aad73931018bd25f84ae4...", "0200000001aad73931018bd25f84ae4..."]],
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
    "method": "combinerawtransaction",
    "params": [["0200000001aad73931018bd25f84ae4...", "0200000001aad73931018bd25f84ae4..."]],
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
```rs
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "combinerawtransaction",
            "params": [["0200000001aad73931018bd25f84ae4...", "0200000001aad73931018bd25f84ae4..."]],
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
    "result": "0200000001aad73931018bd25f84ae400b68d85d7faff77707c1e6b6c6b8b4e2f7..."
}
```

### Response Parameters

| Field  | Type   | Description                                                   |
| ------ | ------ | ------------------------------------------------------------- |
| result | string | The hex-encoded raw transaction with all signatures combined. |

### Use Case

The `combinerawtransaction` method is essential for:

* Multi-signature transaction completion
* Combining signatures from offline signers
* Legacy multi-sig wallet implementations
* Transaction aggregation services
* Batch signing operations
* Compatibility with older signing tools

### Error Handling

| Status Code | Error Message               | Cause                                            |
| ----------- | --------------------------- | ------------------------------------------------ |
| 403         | RBAC: access denied         | Missing or invalid ACCESS-TOKEN.                 |
| -22         | TX decode failed            | One or more transaction hex strings are invalid. |
| -8          | Transactions not compatible | The transactions have different inputs/outputs.  |

### Integration Notes

The `combinerawtransaction` method helps developers:

* Support legacy multi-sig workflows
* Integrate with older hardware wallets
* Build backward-compatible signing solutions
* Migrate from raw transactions to PSBT format
* Handle mixed signing environments
