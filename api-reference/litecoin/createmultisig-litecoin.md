---
description: >-
  Example code for the createmultisig JSON-RPC method. Complete guide on how to
  use createmultisig JSON-RPC in GetBlock Web3 documentation.
---

# createmultisig - Litecoin

Creates a multi-signature address with n signature of m keys required.

### Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| nrequired | number | The number of required signatures out of n keys. |
| keys      | array  | Array of hex-encoded public keys.                |

### Request examples

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "createmultisig",
    "params": [2, ["pubkey1", "pubkey2", "pubkey3"]],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="axios.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "createmultisig",
    "params": [2, ["pubkey1", "pubkey2", "pubkey3"]],
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
{% endcode %}
{% endtab %}

{% tab title="Python (requests)" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "createmultisig",
    "params": [2, ["pubkey1", "pubkey2", "pubkey3"]],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust (reqwest)" %}
{% code title="main.rs" %}
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
            "method": "createmultisig",
            "params": [2, ["pubkey1", "pubkey2", "pubkey3"]],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "address": "multisig_address...",
        "redeemScript": "script...",
        "descriptor": "sh(multi(2,...))#..."
    }
}
```

### Response Parameters

| Field        | Type   | Description                       |
| ------------ | ------ | --------------------------------- |
| address      | string | The multisig address.             |
| redeemScript | string | The redeem script.                |
| descriptor   | string | The descriptor for this multisig. |

### Use Case

The `createmultisig` method is essential for:

* Multisig wallets
* Shared custody
* Escrow services
* Enhanced security

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `createmultisig` method helps developers build robust applications that interact with the Litecoin blockchain.
