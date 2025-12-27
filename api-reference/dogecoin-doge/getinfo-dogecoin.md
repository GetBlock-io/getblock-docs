---
description: >-
  Example code for the getinfo JSON-RPC method. Complete guide on how to use
  getinfo JSON-RPC in GetBlock Web3 documentation.
---

# getinfo - Dogecoin

This method returns an object containing various state info about the node.

{% hint style="info" %}
* Some wallet-related fields may not be available on shared nodes
* The `balance` field will be 0 on nodes without wallet access
{% endhint %}

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getinfo",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getinfo",
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
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getinfo",
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
            "method": "getinfo",
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
    "result": {
        "version": 1140500,
        "protocolversion": 70015,
        "walletversion": 130000,
        "balance": 0.00000000,
        "blocks": 3904788,
        "timeoffset": 0,
        "connections": 8,
        "proxy": "",
        "difficulty": 3525264.838757254,
        "testnet": false,
        "keypoololdest": 1609459200,
        "keypoolsize": 100,
        "paytxfee": 0.00000000,
        "relayfee": 0.00100000,
        "errors": ""
    },
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field           | Type    | Description                            |
| --------------- | ------- | -------------------------------------- |
| version         | number  | The server version.                    |
| protocolversion | number  | The protocol version.                  |
| walletversion   | number  | The wallet version.                    |
| balance         | number  | The total balance (if wallet enabled). |
| blocks          | number  | The current block count.               |
| timeoffset      | number  | The time offset.                       |
| connections     | number  | The number of connections.             |
| proxy           | string  | The proxy used.                        |
| difficulty      | number  | The current difficulty.                |
| testnet         | boolean | If on testnet.                         |
| keypoololdest   | number  | The oldest keypool time.               |
| keypoolsize     | number  | The keypool size.                      |
| paytxfee        | number  | The transaction fee.                   |
| relayfee        | number  | The minimum relay fee.                 |
| errors          | string  | Any errors.                            |

## Use Case

The `getinfo` method is essential for:

* Node status monitoring
* Version verification
* Network information
* Wallet status (if enabled)
* General node health checks

## Error Handling

| Error Code | Message   | Cause                            |
| ---------- | --------- | -------------------------------- |
| 403        | Forbidden | Missing or invalid ACCESS-TOKEN. |
