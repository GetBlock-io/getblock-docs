---
description: >-
  Example code for the getblockchaininfo JSON-RPC method. Complete guide on how
  to use getblockchaininfo JSON-RPC in GetBlock Web3 documentation.
---

# getblockchaininfo - Litecoin

This method returns an object containing various state info regarding blockchain processing.

### Parameters

* None

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
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
{% endcode %}
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="axios.js" %}
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
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

{% code title="Response (example)" %}
```json
{
    "result": {
        "chain": "main",
        "blocks": 3050072,
        "headers": 3050072,
        "bestblockhash": "54b4e01920b72c4257d12cdc90cbee2732498568e9f8dfc23f0b758d22bf4afc",
        "difficulty": 96475280.13079503,
        "mediantime": 1770190296,
        "verificationprogress": 0.9999997778976507,
        "initialblockdownload": false,
        "chainwork": "0000000000000000000000000000000000000000000029bac2285835177ecd5e",
        "size_on_disk": 245266343141,
        "pruned": false,
        "softforks": {
            "bip34": {
                "type": "buried",
                "active": true,
                "height": 710000
            },
            "bip66": {
                "type": "buried",
                "active": true,
                "height": 811879
            },
            "bip65": {
                "type": "buried",
                "active": true,
                "height": 918684
            },
            "csv": {
                "type": "buried",
                "active": true,
                "height": 1201536
            },
            "segwit": {
                "type": "buried",
                "active": true,
                "height": 1201536
            },
            "taproot": {
                "type": "bip8",
                "bip8": {
                    "status": "active",
                    "start_height": 2161152,
                    "timeout_height": 2370816,
                    "since": 2257920
                },
                "height": 2257920,
                "active": true
            },
            "mweb": {
                "type": "bip8",
                "bip8": {
                    "status": "active",
                    "start_height": 2217600,
                    "timeout_height": 2427264,
                    "since": 2265984
                },
                "height": 2265984,
                "active": true
            }
        },
        "warnings": ""
    },
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

### Response Parameters

| Field                | Type    | Description                                           |
| -------------------- | ------- | ----------------------------------------------------- |
| chain                | string  | Current network name (main, test, regtest).           |
| blocks               | number  | The current number of blocks processed in the server. |
| headers              | number  | The current number of headers validated.              |
| bestblockhash        | string  | The hash of the currently best block.                 |
| difficulty           | number  | The current difficulty.                               |
| mediantime           | number  | Median time for the current best block.               |
| verificationprogress | number  | Estimate of verification progress \[0..1].            |
| initialblockdownload | boolean | Whether node is in initial block download mode.       |
| chainwork            | string  | Total amount of work in active chain.                 |
| size\_on\_disk       | number  | Estimated size of the block and undo files on disk.   |
| pruned               | boolean | If the blocks are subject to pruning.                 |
| softforks            | object  | Status of softforks in progress.                      |
| warnings             | string  | Any network and blockchain warnings.                  |

### Use Case

The `getblockchaininfo` method is essential for:

* Monitoring node synchronization status
* Checking chain health and progress
* Verifying softfork activation status
* Determining if node is fully synced
* Building status dashboards

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `getblockchaininfo` method helps developers:

* Display chain status in applications
* Check MWEB activation status
* Monitor initial block download
* Verify node health
* Track softfork deployments
