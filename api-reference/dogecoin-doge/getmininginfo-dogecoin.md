---
description: >-
  Example code for the getmininginfo JSON-RPC method. Complete guide on how to
  use getmininginfo JSON-RPC in GetBlock Web3 documentation.
---

# getmininginfo - Dogecoin

This method returns an object containing mining-related information.

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
    "method": "getmininginfo",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="getmininginfo.js" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getmininginfo",
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
{% code title="getmininginfo.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getmininginfo",
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
{% code title="getmininginfo.rs" %}
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
            "method": "getmininginfo",
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

```json
{
    "result": {
        "blocks": 3904788,
        "currentblocksize": 1000,
        "currentblocktx": 0,
        "difficulty": 3525264.838757254,
        "errors": "",
        "genproclimit": -1,
        "networkhashps": 280000000000000,
        "pooledtx": 5,
        "testnet": false,
        "chain": "main",
        "generate": false
    },
    "error": null,
    "id": "getblock.io"
}
```

## Response Parameters

| Field            | Type    | Description                                            |
| ---------------- | ------- | ------------------------------------------------------ |
| blocks           | number  | The current block height.                              |
| currentblocksize | number  | The size of the block being mined.                     |
| currentblocktx   | number  | The number of block transactions being mined.          |
| difficulty       | number  | The current mining difficulty.                         |
| errors           | string  | Any network or blockchain errors.                      |
| genproclimit     | number  | The processor limit for generation (-1 for unlimited). |
| networkhashps    | number  | The estimated network hashes per second.               |
| pooledtx         | number  | The size of the mempool.                               |
| testnet          | boolean | If on testnet.                                         |
| chain            | string  | Current network (main/test/regtest).                   |
| generate         | boolean | If generation is enabled.                              |

## Use Case

The `getmininginfo` method is essential for:

* Mining pool dashboards
* Network hashrate monitoring
* Mining profitability calculations
* Blockchain statistics
* Network health monitoring

## Error Handling

| Error Code | Message   | Cause                            |
| ---------- | --------- | -------------------------------- |
| 403        | Forbidden | Missing or invalid ACCESS-TOKEN. |
