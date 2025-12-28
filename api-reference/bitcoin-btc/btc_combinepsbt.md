---
description: >-
  Example code for the combinepsbt JSON RPC method. Сomplete guide on how to use
  combinepsbt JSON RPC in GetBlock Web3 documentation.
---

# combinepsbt - Bitcoin

This method combines multiple partially signed Bitcoin transactions (PSBTs) into one transaction. All PSBTs must be for the same transaction (same inputs and outputs).

### Parameters

| Parameter | Type  | Required | Description                                                  |
| --------- | ----- | -------- | ------------------------------------------------------------ |
| txs       | array | Yes      | An array of base64 strings of partially signed transactions. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "combinepsbt",
    "params": [["cHNidP8BAHUCAAAAASaBcTce...", "cHNidP8BAHUCAAAAASaBcTce..."]],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "combinepsbt",
    "params": [["cHNidP8BAHUCAAAAASaBcTce...", "cHNidP8BAHUCAAAAASaBcTce..."]],
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
    "method": "combinepsbt",
    "params": [["cHNidP8BAHUCAAAAASaBcTce...", "cHNidP8BAHUCAAAAASaBcTce..."]],
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
            "method": "combinepsbt",
            "params": [["cHNidP8BAHUCAAAAASaBcTce...", "cHNidP8BAHUCAAAAASaBcTce..."]],
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
    "result": "cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cezh..."
}
```

### Response Parameters

| Field  | Type   | Description                                                                   |
| ------ | ------ | ----------------------------------------------------------------------------- |
| result | string | The base64-encoded partially signed transaction with all signatures combined. |

### Use Case

The `combinepsbt` method is essential for:

* Multi-signature wallet implementations
* Combining signatures from multiple hardware wallets
* Collaborative transaction signing workflows
* CoinJoin transaction construction
* Escrow services requiring multiple signers
* Corporate treasury management with multiple approvers

### Error Handling

| Status Code | Error Message        | Cause                            |
| ----------- | -------------------- | -------------------------------- |
| 403         | RBAC: Access denied  | Missing or invalid ACCESS-TOKEN. |
| -22         | Invalid params       | The params array is malformed.   |
| -1          | Missing param        | the param array is empty         |

### Integration Notes

The `combinepsbt` method helps developers:

* Build multi-party signing protocols
* Implement threshold signature schemes
* Create collaborative custody solutions
* Support air-gapped hardware wallet workflows
* Enable distributed key management systems
