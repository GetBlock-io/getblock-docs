---
description: >-
  Example code for the eth_uninstallFilter JSON-RPC method. Complete guide on
  how to use eth_uninstallFilter JSON-RPC in GetBlock Web3 documentation.
---

# eth\_uninstallFilter - Mantle

This method uninstalls a filter with given ID. It should always be called when a watch is no longer needed. Filters timeout when not requested with `eth_getFilterChanges` for a period of time.

### Parameters

| Parameter | Type   | Description                |
| --------- | ------ | -------------------------- |
| filterId  | string | The filter ID to uninstall |

### Request examples

{% tabs %}
{% tab title="curl" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_uninstallFilter",
    "params": ["0x1a2b3c4d5e6f"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="uninstallFilter.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_uninstallFilter",
    "params": ["0x1a2b3c4d5e6f"],
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
{% code title="uninstall_filter.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_uninstallFilter",
    "params": ["0x1a2b3c4d5e6f"],
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
            "method": "eth_uninstallFilter",
            "params": ["0x1a2b3c4d5e6f"],
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
    "result": true
}
```

### Response Parameters

| Field  | Type    | Description                                 |
| ------ | ------- | ------------------------------------------- |
| result | boolean | True if filter was successfully uninstalled |

### Use Case

The `eth_uninstallFilter` method is essential for:

* Cleaning up unused filters
* Resource management
* Proper filter lifecycle handling
* Preventing filter timeout issues

### Error Handling

| Status Code | Error Message | Cause                           |
| ----------- | ------------- | ------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const success = await provider.send('eth_uninstallFilter', ['0x1a2b3c4d5e6f']);
console.log('Filter uninstalled:', success);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const success = await client.request({
    method: 'eth_uninstallFilter',
    params: ['0x1a2b3c4d5e6f']
});
console.log('Filter uninstalled:', success);
```
{% endcode %}
{% endtab %}
{% endtabs %}
