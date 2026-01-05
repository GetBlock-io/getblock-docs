---
description: >-
  Example code for the getNetwork JSON-RPC method. Complete guide on how to use
  getNetwork JSON-RPC in GetBlock Web3 documentation.
---

# getNetwork - Stellar

This method returns network configuration information for the Stellar network the node is connected to.

### Parameters

* None

### Request examples

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getNetwork",
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="getNetwork.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getNetwork",
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

{% tab title="Python" %}
{% code title="getNetwork.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getNetwork",
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

{% tab title="Rust" %}
{% code title="getNetwork.rs" %}
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
            "method": "getNetwork",
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
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "passphrase": "Public Global Stellar Network ; September 2015",
        "protocolVersion": 24
    }
}
```

### Response Parameters

| Field           | Type    | Description                                |
| --------------- | ------- | ------------------------------------------ |
| passphrase      | string  | Network passphrase for transaction signing |
| protocolVersion | integer | Current protocol version of the network    |

### Use Case

The `getNetwork` method is essential for:

* Network identification and validation
* Transaction signing configuration
* Multi-network application routing
* Testnet vs mainnet detection
* Protocol version verification
* SDK configuration

### Error Handling

| Status Code | Error Message | Cause                           |
| ----------- | ------------- | ------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN |
