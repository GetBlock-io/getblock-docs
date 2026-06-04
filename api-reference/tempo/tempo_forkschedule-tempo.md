---
description: >-
  Example code for the tempo_forkSchedule JSON-RPC method. Complete guide on how
  to use tempo_forkSchedule JSON-RPC in GetBlock Web3 documentation.
---

# tempo\_forkSchedule - Tempo

Returns the Tempo fork schedule and the currently active fork at the chain head. Each entry includes the fork name, activation timestamp, whether it is active, and an EIP-2124 fork hash. Available on all Tempo nodes.

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
    "method": "tempo_forkSchedule",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "tempo_forkSchedule",
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
    "method": "tempo_forkSchedule",
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

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "tempo_forkSchedule",
        "params": [],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "schedule": [
            {
                "name": "T0",
                "activationTime": 0,
                "active": true,
                "forkId": "0xfde57c3e"
            },
            {
                "name": "T1",
                "activationTime": 1770908400,
                "active": true,
                "forkId": "0x9e6fe384"
            },
            {
                "name": "T1A",
                "activationTime": 1770908400,
                "active": true,
                "forkId": "0x9e6fe384"
            },
            {
                "name": "T1B",
                "activationTime": 1771858800,
                "active": true,
                "forkId": "0x73a4f670"
            },
            {
                "name": "T1C",
                "activationTime": 1773327600,
                "active": true,
                "forkId": "0x2a3ee80d"
            },
            {
                "name": "T2",
                "activationTime": 1774965600,
                "active": true,
                "forkId": "0x471a451c"
            },
            {
                "name": "T3",
                "activationTime": 1777298400,
                "active": true,
                "forkId": "0xd2087b77"
            }
        ],
        "active": "T3"
    }
}
```
{% endcode %}

## Response Parameters

| Field                             | Type              | Description                                                    |
| --------------------------------- | ----------------- | -------------------------------------------------------------- |
| result.schedule                   | array of ForkInfo | Ordered list of Tempo forks                                    |
| result.schedule\[].name           | string            | Fork name (e.g. `T0`, `T1`, `T2`, `T3`)                        |
| result.schedule\[].activationTime | integer           | Unix timestamp of fork activation                              |
| result.schedule\[].active         | boolean           | Whether this fork is active at the chain head                  |
| result.schedule\[].forkId         | string            | EIP-2124 fork hash (omitted for forks that are not yet active) |
| result.active                     | string            | Name of the currently active fork at the chain head            |

## Use Cases

* Detecting which protocol upgrades are active on Tempo
* Light clients computing EIP-2124 fork identifiers for handshake
* Tooling that needs to gate features by activation timestamp

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'tempo_forkSchedule',
    params: []
}});
console.log(response.data);
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests;

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'tempo_forkSchedule',
    'params': []
}});
print(response.json());
```
{% endtab %}
{% endtabs %}
