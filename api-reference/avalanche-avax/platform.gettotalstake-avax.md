---
description: >-
  Example code for the platform.getTotalStake JSON-RPC method. Complete guide on
  how to use platform.getTotalStake JSON-RPC in GetBlock Web3 documentation.
---

# platform.getTotalStake - AVAX

Returns the total amount of AVAX staked on a given subnet, in nAVAX. For the Primary Network, this is the network's total stake securing consensus.

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
    "method": "platform.getTotalStake",
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
    "method": "platform.getTotalStake",
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
    "method": "platform.getTotalStake",
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
        "method": "platform.getTotalStake",
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
        "stake": "275000000000000000",
        "weight": "275000000000000000"
    }
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                                      |
| ------------- | ------ | ------------------------------------------------ |
| result.stake  | string | Total stake in nAVAX (decimal string)            |
| result.weight | string | Total weight (equal to stake on Primary Network) |

## Use Cases

* Network security dashboards
* Computing staking rewards APR estimates
* Tracking subnet adoption by stake weight

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`         |
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
    method: 'platform.getTotalStake',
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
    'method': 'platform.getTotalStake',
    'params': {}
}}
response = requests.post(url, json=payload)
print(response.json())
```
{% endtab %}
{% endtabs %}
