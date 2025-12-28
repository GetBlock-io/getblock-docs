---
description: >-
  Example code for the converttopsbt JSON RPC method. Сomplete guide on how to
  use converttopsbt JSON RPC in GetBlock Web3 documentation.
---

# converttopsbt - Bitcoin

This method converts a network-serialized transaction to a PSBT (Partially Signed Bitcoin Transaction). This is useful for taking an unsigned or partially signed raw transaction and converting it to the PSBT format.

### Parameters

| Parameter     | Type    | Required | Description                                                             |
| ------------- | ------- | -------- | ----------------------------------------------------------------------- |
| hexstring     | string  | Yes      | The hex string of a raw transaction.                                    |
| permitsigdata | boolean | No       | If true, any signatures in the input will be retained (default: false). |
| iswitness     | boolean | No       | Whether the transaction hex is a witness serialized transaction.        |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "converttopsbt",
    "params": ["0200000001aad73931018bd25f84ae400b68d85d7f...", false, null],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "converttopsbt",
    "params": ["0200000001aad73931018bd25f84ae400b68d85d7f...", false, null],
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
    "method": "converttopsbt",
    "params": ["0200000001aad73931018bd25f84ae400b68d85d7f...", False, None],
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
            "method": "converttopsbt",
            "params": ["0200000001aad73931018bd25f84ae400b68d85d7f...", false, null],
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

| Field  | Type   | Description                          |
| ------ | ------ | ------------------------------------ |
| result | string | The resulting PSBT in base64 format. |

### Use Case

The `converttopsbt` method is essential for:

* Migrating legacy transactions to PSBT format
* Enabling hardware wallet compatibility
* Converting unsigned transactions for multi-sig workflows
* Supporting modern signing protocols
* Integrating with PSBT-aware applications
* Bridging between old and new transaction formats

### Error Handling

| Status Code | Error Message                   | Cause                                         |
| ----------- | ------------------------------- | --------------------------------------------- |
| 403         | Forbidden                       | Missing or invalid ACCESS-TOKEN.              |
| -22         | TX decode failed                | The transaction hex string is malformed.      |
| -22         | Inputs must not have scriptSigs | Signed inputs found with permitsigdata=false. |

### Integration Notes

The `converttopsbt` method helps developers:

* Upgrade legacy transaction workflows
* Support both raw and PSBT transaction formats
* Enable offline signing with hardware wallets
* Build modern multi-signature solutions
* Maintain backward compatibility with older systems
