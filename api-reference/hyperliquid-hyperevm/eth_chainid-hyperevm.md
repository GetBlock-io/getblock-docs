---
description: >-
  Example code for the eth_chainId JSON RPC method. Сomplete guide on how to use
  eth_chainId JSON RPC in GetBlock Web3 documentation.
---

# eth\_chainId - HyperEVM

This method gets the chain ID of the current network.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';
let data = JSON.stringify({
  "method": "eth_blockNumber",
  "id": "getblock.io",
  "jsonrpc": "2.0"
});

let config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>',
  headers: { 
    'Content-Type': 'application/json'
  },
  data : data
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});

```
{% endtab %}

{% tab title="Request" %}
```py
import requests
import json

url = "https://go.getblock.io/<ACCESS_TOKEN>"

payload = json.dumps({
  "method": "eth_blockNumber",
  "id": "getblock.io",
  "jsonrpc": "2.0"
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" %}
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
            "method": "eth_chainId",
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

{% code title="Response (JSON)" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x3e7"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description           |
| ------ | ------ | --------------------- |
| result | string | Hexadecimal chain ID. |

## Use Case

The `eth_chainId` method is essential for:

* Network verification before transactions
* Wallet connection validation
* EIP-155 transaction signing
* Multi-network application routing
* Preventing replay attacks across chains

## Error Handling

| Error Code | Message        | Cause       |
| ---------- | -------------- | ----------- |
| -32603     | Internal error | Node issue. |

### Web3 Integration

{% include "../../.gitbook/includes/the-correct-ether-and-viem.md" %}
