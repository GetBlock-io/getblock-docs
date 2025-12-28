---
description: >-
  Example code for the eth_blockNumber JSON RPC method. Сomplete guide on how to
  use eth_blockNumber JSON RPC in GetBlock Web3 documentation.
---

# eth\_blockNumber - HyperEVM

This methods get the number of the most recent block.

#### Paramaters&#x20;

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    ""method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
   "method": "eth_blockNumber",
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

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
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

{% tab title="Rust" %}
{% code overflow="wrap" %}
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
            "method": "eth_blockNumber",
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
    "result": "0x1a2b3c"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                        |
| ------ | ------ | -------------------------------------------------- |
| result | string | Hexadecimal block number of the most recent block. |

## Use Case

The `eth_blockNumber` method is essential for:

* Monitoring chain progress and health
* Synchronization tracking
* Block confirmation calculations
* Event indexing starting points
* Health checks and monitoring dashboards

## Error Handling

| Error Code | Message        | Cause                       |
| ---------- | -------------- | --------------------------- |
| -32603     | Internal error | Node synchronization issue. |

### Web3 Integration

{% include "../../.gitbook/includes/const-ethers-require-....md" %}
