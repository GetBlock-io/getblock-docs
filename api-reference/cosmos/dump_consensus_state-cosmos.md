---
description: >-
  Example code for the dump_consensus_state JSON-RPC method. Complete guide on
  how to use dump_consensus_state JSON-RPC in GetBlock Web3 documentation.
---

# dump\_consensus\_state - Cosmos

Returns the full consensus state including the full validator set, all votes, and connected peers. Heavier than `consensus_state` — use sparingly.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/dump_consensus_state' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "dump_consensus_state",
    "params": [],
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/dump_consensus_state'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "dump_consensus_state",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/dump_consensus_state',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/dump_consensus_state"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "dump_consensus_state",
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
        "method": "dump_consensus_state",
        "params": [],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/dump_consensus_state")
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
        "round_state": {
            "height": "23456789",
            "round": 0,
            "step": 1,
            "start_time": "2026-05-26T13:42:18.5273Z",
            "validators": {
                "validators": []
            },
            "votes": []
        },
        "peers": []
    }
}
```
{% endcode %}

## Response Parameters

| Field               | Type   | Description                                         |
| ------------------- | ------ | --------------------------------------------------- |
| result.round\_state | object | Detailed round state including validators and votes |
| result.peers        | array  | Peer-level consensus state snapshots                |

## Use Cases

* Deep diagnostics during consensus issues
* Forensic analysis of past consensus rounds
* Validator post-mortems

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 404         | Not found         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="CosmJS" %}
```javascript
import { Tendermint37Client, HttpClient } from '@cosmjs/tendermint-rpc';

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/dump_consensus_state');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "dump_consensus_state", "params": []});
console.log(result);
```
{% endtab %}

{% tab title="cosmpy / requests" %}
```python
# cosmpy wraps Cosmos REST / gRPC. For raw CometBFT RPC access, use requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/dump_consensus_state', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'dump_consensus_state',
    'params': []
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
