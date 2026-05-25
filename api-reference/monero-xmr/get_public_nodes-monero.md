# get\_public\_nodes monero

This non-JSON-RPC endpoint returns peers that have explicitly advertised themselves as public RPC nodes (those running with `--public-node`).

## Parameters

| Parameter | Type    | Required | Description                               |
| --------- | ------- | -------- | ----------------------------------------- |
| white     | boolean | No       | Include white-list peers (default `true`) |
| gray      | boolean | No       | Include gray-list peers (default `false`) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/get_public_nodes' \
--header 'Content-Type: application/json' \
--data-raw '{
    "white": true,
    "gray": false
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "white": true,
    "gray": false
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/get_public_nodes',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/get_public_nodes"

payload = json.dumps({
    "white": true,
    "gray": false
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
        "white": true,
        "gray": false
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/get_public_nodes")
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
    "white": [
        {
            "host": "node.example.com",
            "last_seen": 1737869732,
            "rpc_credits_per_hash": 0,
            "rpc_port": 18089
        }
    ],
    "status": "OK",
    "untrusted": false
}
```
{% endcode %}

## Response Parameters

| Field                            | Type         | Description                                                      |
| -------------------------------- | ------------ | ---------------------------------------------------------------- |
| white                            | array        | Public RPC peers from the white list                             |
| gray                             | array        | Public RPC peers from the gray list (when requested)             |
| white\[].host                    | string       | Hostname or IP                                                   |
| white\[].rpc\_port               | unsigned int | Advertised RPC port                                              |
| white\[].rpc\_credits\_per\_hash | unsigned int | RPC payment credits charged per PoW hash (if RPC pay is enabled) |

## Use Cases

* Discovering community-run public RPC endpoints
* Bootstrapping wallets that need a remote daemon
* Building decentralized RPC aggregators

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
// Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
// Use a plain HTTP client as shown in the Request Example above.
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/get_public_nodes', {"white": true, "gray": false});
console.log(response.data);
```
{% endtab %}

{% tab title="monero-python" %}
```python
# Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
# Use a plain HTTP client as shown in the Request Example above.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/get_public_nodes', json={"white": true, "gray": false})
print(response.json())
```
{% endtab %}
{% endtabs %}
