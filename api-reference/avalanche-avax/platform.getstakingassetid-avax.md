---
description: >-
  Example code for the platform.getStakingAssetID JSON-RPC method. Complete
  guide on how to use platform.getStakingAssetID JSON-RPC in GetBlock Web3
  documentation.
---

# platform.getStakingAssetID - AVAX

Returns the asset ID used for staking on a given subnet. For the Primary Network this is the AVAX asset ID. Subnet L1s may use custom staking assets.

## Parameters

| Parameter | Type   | Required | Description                          |
| --------- | ------ | -------- | ------------------------------------ |
| subnetID  | string | No       | Subnet ID (default: Primary Network) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "platform.getStakingAssetID",
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
    "method": "platform.getStakingAssetID",
    "params": {},
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "platform.getStakingAssetID",
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
        "method": "platform.getStakingAssetID",
        "params": {},
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P")
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "assetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z"
    }
}
```
{% endcode %}

## Response Parameters

| Field          | Type   | Description                                                             |
| -------------- | ------ | ----------------------------------------------------------------------- |
| result.assetID | string | Asset ID (Base58 CB58-encoded) used for staking on the requested subnet |

## Use Cases

* Subnet integrations that need to know the staking token
* Wallet support for custom-token-staked L1s

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="AvalancheJS / Axios" %}
```javascript
// AvalancheJS provides typed clients for P-Chain and X-Chain.
// P-Chain operations are exposed through `pvm` in AvalancheJS.
// For raw JSON-RPC access, use a generic HTTP client as in the Request Example.
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'platform.getStakingAssetID',
    params: {}
}});
console.log(response.data);
```
{% endtab %}

{% tab title="Python" %}
```python
# AvalancheJS is JavaScript-only. For Python, use a generic JSON-RPC client.
import requests
import json

url = 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P'
payload = {
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'platform.getStakingAssetID',
    'params': {}
}}
response = requests.post(url, json=payload)
print(response.json())
```
{% endtab %}
{% endtabs %}
