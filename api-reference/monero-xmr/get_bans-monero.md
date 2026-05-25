# get\_bans monero

{% hint style="danger" %}
**This method is `{disallowed}` on GetBlock shared endpoints.**

This is an administrative or state-mutating operation that GetBlock blocks on shared and public infrastructure for safety. Calling this method will return an error. If you need access for monitoring, mining-pool operations, or other legitimate use cases, contact GetBlock support about a dedicated node configuration that exposes this method.
{% endhint %}

This method returns the list of currently banned IP addresses on the node along with the time remaining on each ban.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_bans",
    "params": {},
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "get_bans",
    "params": {},
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
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "get_bans",
    "params": {},
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "get_bans",
        "params": {},
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "bans": [
            {
                "host": "192.0.2.1",
                "ip": 16777343,
                "seconds": 3600
            }
        ],
        "status": "OK"
    }
}
```
{% endcode %}

## Response Parameters

| Field                  | Type         | Description                  |
| ---------------------- | ------------ | ---------------------------- |
| result.bans            | array        | List of active bans          |
| result.bans\[].host    | string       | Banned host (IP or hostname) |
| result.bans\[].seconds | unsigned int | Seconds remaining on the ban |

## Use Cases

* Reviewing ban state on a dedicated node
* Detecting persistent bad-actor IPs

## Error Handling

| Status Code | Error Message     | Cause                                                                            |
| ----------- | ----------------- | -------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                              |
| -32602      | Invalid params    | Request parameters are missing or malformed                                      |
| -32601      | Method not found  | Method does not exist or is not enabled on this node                             |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                |
| -32601      | Method disabled   | Disallowed on GetBlock shared endpoints; available on dedicated nodes by request |

## SDK Integration

Since this method is disallowed on shared endpoints, SDK examples are omitted. If you have a dedicated node, use the same JSON-RPC body structure shown in the Request Example.
