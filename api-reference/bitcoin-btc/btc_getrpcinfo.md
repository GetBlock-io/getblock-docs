---
description: >-
  Example code for the getrpcinfo JSON-RPC method. Complete guide on how to use
  getrpcinfo JSON-RPC in GetBlock Web3 documentation.
---

# getrpcinfo - Bitcoin

This method returns details about the RPC server.

### Parameters

* none

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getrpcinfo",
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
    "method": "getrpcinfo",
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
    "method": "getrpcinfo",
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
            "method": "getrpcinfo",
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
        "active_commands": [
            {
                "method": "getblock",
                "duration": 78931
            },
            {
                "method": "getrpcinfo",
                "duration": 2860
            }
        ],
        "logpath": "/home/bitcoin/.bitcoin/debug.log"
    },
    "error": null,
    "id": "getblock.io"
}
```

### Response Parameters

| Field                        | Type   | Description                              |
| ---------------------------- | ------ | ---------------------------------------- |
| active\_commands             | array  | Currently active RPC commands.           |
| active\_commands\[].method   | string | Name of the RPC command.                 |
| active\_commands\[].duration | number | Duration of the command in microseconds. |
| logpath                      | string | Path to the debug log file.              |

### Use Case

The `getrpcinfo` method is essential for:

* Monitoring active RPC commands
* Debugging long-running requests
* Building administrative dashboards
* Performance analysis
* Troubleshooting RPC issues
* Understanding server load

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration With Web3

The `getrpcinfo` method helps developers:

* Monitor RPC server activity
* Identify long-running commands
* Build operational monitoring
* Diagnose performance bottlenecks
* Track concurrent request patterns
