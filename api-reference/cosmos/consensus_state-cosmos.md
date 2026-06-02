---
description: >-
  Example code for the consensus_state JSON-RPC method. Complete guide on how to
  use consensus_state JSON-RPC in GetBlock Web3 documentation.
---

# consensus\_state - Cosmos

Returns the current consensus state — the round, step, and votes the node has observed. Useful for diagnosing consensus stalls or observing the consensus process in real time.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "consensus_state",
    "params": [],
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/consensus_state'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "consensus_state",
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
    "method": "consensus_state",
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
        "method": "consensus_state",
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
        "round_state": {
            "height/round/step": "23456789/0/1",
            "start_time": "2026-05-26T13:42:18.5273Z",
            "proposal_block_hash": "6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0",
            "locked_block_hash": "",
            "valid_block_hash": "",
            "height_vote_set": []
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                                     | Type   | Description                                                            |
| ----------------------------------------- | ------ | ---------------------------------------------------------------------- |
| result.round\_state\['height/round/step'] | string | Current height, round, and step in the consensus state machine         |
| result.round\_state.start\_time           | string | ISO 8601 timestamp when the current round began                        |
| result.round\_state.proposal\_block\_hash | string | Hash of the currently proposed block (may be empty if no proposal yet) |
| result.round\_state.height\_vote\_set     | array  | Per-round vote sets                                                    |

## Use Cases

* Diagnosing consensus halts
* Validator operations monitoring
* Research into consensus liveness

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="CosmJS" %}
```javascript
import { Tendermint37Client, HttpClient } from '@cosmjs/tendermint-rpc';

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/consensus_params');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "consensus_state", "params": []});
console.log(result);
```
{% endtab %}

{% tab title="cosmpy / requests" %}
```python
# cosmpy wraps Cosmos REST / gRPC. For raw CometBFT RPC access, use requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/consensus_params', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'consensus_state',
    'params': []
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
