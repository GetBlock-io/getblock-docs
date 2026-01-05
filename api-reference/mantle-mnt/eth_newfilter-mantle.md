---
description: >-
  Example code for the eth_newFilter JSON-RPC method. Complete guide on how to
  use eth_newFilter JSON-RPC in GetBlock Web3 documentation.
---

# eth\_newFilter - Mantle

This method creates a filter object to notify when state changes (logs). To check if the state has changed, call eth\_getFilterChanges.

### Parameters

| Parameter    | Type   | Description           |
| ------------ | ------ | --------------------- |
| filterObject | object | Filter options object |

Filter Object:

| Field     | Type         | Description                    |
| --------- | ------------ | ------------------------------ |
| fromBlock | string       | Starting block (hex or tag)    |
| toBlock   | string       | Ending block (hex or tag)      |
| address   | string/array | Contract address(es) to filter |
| topics    | array        | Array of topic filters         |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_newFilter",
    "params": [{
        "fromBlock": "0x3F6700",
        "toBlock": "0x3F6800",
        "address": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
        "topics": []
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_newFilter",
    "params": [{
        "fromBlock": "0x3F6700",
        "toBlock": "0x3F6800",
        "address": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
        "topics": []
    }],
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
{% code title="Python (requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_newFilter",
    "params": [{
        "fromBlock": "0x3F6700",
        "toBlock": "0x3F6800",
        "address": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
        "topics": []
    }],
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
{% code title="Rust (reqwest)" %}
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
            "method": "eth_newFilter",
            "params": [{
                "fromBlock": "0x3F6700",
                "toBlock": "0x3F6800",
                "address": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
                "topics": []
            }],
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

{% code title="Response (JSON)" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1a2b3c4d5e6f"
}
```
{% endcode %}

### Response Parameters

| Field  | Type   | Description                                 |
| ------ | ------ | ------------------------------------------- |
| result | string | Filter ID to use with eth\_getFilterChanges |

### Use Case

The `eth_newFilter` method is essential for:

* Event monitoring without WebSocket
* Log polling applications
* Contract event tracking
* Backend services monitoring events
* DApp event listeners

### Error Handling

| Status Code | Error Message  | Cause                           |
| ----------- | -------------- | ------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params | Invalid filter parameters       |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const filterId = await provider.send('eth_newFilter', [{
    fromBlock: '0x3F6700',
    toBlock: '0x3F6800',
    address: '0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE'
}]);
console.log('Filter ID:', filterId);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const filterId = await client.request({
    method: 'eth_newFilter',
    params: [{
        fromBlock: '0x3F6700',
        toBlock: '0x3F6800',
        address: '0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE'
    }]
});
console.log('Filter ID:', filterId);
```
{% endcode %}
{% endtab %}
{% endtabs %}
