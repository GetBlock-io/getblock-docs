---
description: >-
  Example code for the getblockchaininfo JSON RPC method. Сomplete guide on how
  to use getblockchaininfo JSON RPC in GetBlock Web3 documentation.
---

# getblockchaininfo - Bitcoin

This method returns an object containing various state info regarding blockchain processing.

### Parameters

* None

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockchaininfo",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getblockchaininfo",
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
{% endtab %}

{% tab title="Request" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getblockchaininfo",
    "params": [],
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
            "method": "getblockchaininfo",
            "params": [],
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
    "result": {
        "chain": "main",
        "blocks": 928556,
        "headers": 928556,
        "bestblockhash": "000000000000000000006d2c2d0fd10aad6475f2e93698cc2618d78a0668d06e",
        "difficulty": 148195306640204.7,
        "time": 1766147618,
        "mediantime": 1766146969,
        "verificationprogress": 0.999996294840767,
        "initialblockdownload": false,
        "chainwork": "0000000000000000000000000000000000000000ff4d32eb9db38dc0f4b538e7",
        "size_on_disk": 805983801428,
        "pruned": false,
        "warnings": ""
    },
    "error": null
}
```

### Response Parameters

| Field                | Type    | Description                                     |
| -------------------- | ------- | ----------------------------------------------- |
| chain                | string  | Current network name (main, test, regtest).     |
| blocks               | number  | Number of processed blocks.                     |
| headers              | number  | Number of validated headers.                    |
| bestblockhash        | string  | Hash of the current best block.                 |
| difficulty           | number  | Current difficulty.                             |
| time                 | number  | Timestamp of the current best block.            |
| mediantime           | number  | Median time for the current best block.         |
| verificationprogress | number  | Estimate of verification progress (0 to 1).     |
| initialblockdownload | boolean | Whether initial block download is in progress.  |
| chainwork            | string  | Total amount of work in active chain.           |
| size\_on\_disk       | number  | Estimated size of block and undo files on disk. |
| pruned               | boolean | Whether the blocks are subject to pruning.      |
| warnings             | string  | Any network and blockchain warnings.            |
| softforks            | object  | Status of softforks.                            |

### Use Case

The `getblockchaininfo` method is essential for:

* Monitoring node synchronization status
* Checking network and chain parameters
* Tracking softfork activation status
* Building node health dashboards
* Verifying chain state before operations
* Monitoring blockchain warnings

### Error Handling

| Status Code | Error Message       | Cause                            |
| ----------- | ------------------- | -------------------------------- |
| 403         | RBAC: access denied | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `getblockchaininfo` method helps developers:

* Build node monitoring dashboards
* Check sync status before transactions
* Track protocol upgrades
* Monitor chain health
* Implement connection status displays
