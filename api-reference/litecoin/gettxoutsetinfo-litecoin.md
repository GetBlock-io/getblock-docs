---
description: >-
  Example code for the gettxoutsetinfo JSON-RPC method. Complete guide on how to
  use gettxoutsetinfo JSON-RPC in GetBlock Web3 documentation.
---

# gettxoutsetinfo - Litecoin

Returns statistics about the unspent transaction output (UTXO) set.

## Parameters

* None

## Request examples

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettxoutsetinfo",
    "params": [],
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
    "method": "gettxoutsetinfo",
    "params": [],
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
    "method": "gettxoutsetinfo",
    "params": [],
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
            "method": "gettxoutsetinfo",
            "params": [],
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

## Response

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "height": 2750000,
        "bestblock": "blockhash...",
        "transactions": 12345678,
        "txouts": 23456789,
        "bogosize": 1234567890,
        "hash_serialized_2": "hash...",
        "disk_size": 1234567890,
        "total_amount": 70000000.00000000
    }
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                                  |
| ------------- | ------ | -------------------------------------------- |
| height        | number | The current block height.                    |
| transactions  | number | Number of transactions with unspent outputs. |
| txouts        | number | Number of unspent transaction outputs.       |
| total\_amount | number | The total amount in LTC.                     |

## Use Case

{% hint style="info" %}
The `gettxoutsetinfo` method is essential for:

* UTXO set analysis
* Supply verification
* Database statistics
* Chain state monitoring
{% endhint %}

## Error Handling

<details>

<summary>Common error</summary>

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

</details>

## Integration Notes

The `gettxoutsetinfo` method helps developers build robust applications that interact with the Litecoin blockchain.
