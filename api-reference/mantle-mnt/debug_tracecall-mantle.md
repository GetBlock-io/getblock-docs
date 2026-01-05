---
description: >-
  Example code for the debug_traceCall JSON-RPC method. Complete guide on how to
  use debug_traceCall JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceCall - Mantle

This method traces a call without creating a transaction. Similar to eth\_call but provides full EVM execution trace for debugging purposes.

### Parameters

| Parameter    | Type   | Description                     |
| ------------ | ------ | ------------------------------- |
| callObject   | object | Transaction call object         |
| blockNumber  | string | Block number (hex) or tag       |
| tracerConfig | object | (optional) Tracer configuration |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceCall",
    "params": [
        {
            "from": null,
            "to": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
            "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
        },
        "latest",
        {"tracer": "callTracer"}
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "debug_traceCall",
    "params": [
        {
            "from": null,
            "to": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
            "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
        },
        "latest",
        {"tracer": "callTracer"}
    ],
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
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "debug_traceCall",
    "params": [
        {
            "from": None,
            "to": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
            "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
        },
        "latest",
        {"tracer": "callTracer"}
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
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
            "method": "debug_traceCall",
            "params": [
                {
                    "from": null,
                    "to": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
                    "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
                },
                "latest",
                {"tracer": "callTracer"}
            ],
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
    "result": {
        "type": "CALL",
        "from": "0x0000000000000000000000000000000000000000",
        "to": "0x201eba5cc46d216ce6dc03f6a759e8e766e956ae",
        "value": "0x0",
        "gas": "0x2faf080",
        "gasUsed": "0x5c4",
        "input": "0x70a08231...",
        "output": "0x0000000000000000000000000000000000000000000000000000000000000000"
    }
}
```

### Response Parameters

| Field   | Type   | Description        |
| ------- | ------ | ------------------ |
| type    | string | Call type          |
| from    | string | Sender address     |
| to      | string | Recipient address  |
| gas     | string | Gas provided (hex) |
| gasUsed | string | Gas used (hex)     |
| input   | string | Call input data    |
| output  | string | Call output data   |

### Use Case

The `debug_traceCall` method is essential for:

* Contract call debugging without transaction
* Gas estimation analysis
* Smart contract testing
* Understanding call behavior
* Pre-transaction simulation

### Error Handling

| Status Code | Error Message      | Cause                           |
| ----------- | ------------------ | ------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN |
| -32000      | Execution reverted | Contract execution failure      |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const trace = await provider.send('debug_traceCall', [
    {
        to: '0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE',
        data: '0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE'
    },
    'latest',
    { tracer: 'callTracer' }
]);
console.log('Call Trace:', trace);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const trace = await client.request({
    method: 'debug_traceCall',
    params: [
        {
            to: '0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE',
            data: '0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE'
        },
        'latest',
        { tracer: 'callTracer' }
    ]
});
console.log('Call Trace:', trace);
```
{% endtab %}
{% endtabs %}
