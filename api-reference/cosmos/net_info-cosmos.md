---
description: >-
  Example code for the net_info JSON-RPC method. Complete guide on how to use
  net_info JSON-RPC in GetBlock Web3 documentation.
---

# net\_info - Cosmos

Returns information about the node's network — number of peers, listening status, and per-peer connection details.

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
    "method": "net_info",
    "params": [],
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/net_info'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "net_info",
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
    "method": "net_info",
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
        "method": "net_info",
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
        "listening": true,
        "listeners": [
            "Listener(@)"
        ],
        "n_peers": "45",
        "peers": [
            {
                "node_info": {
                    "id": "b93270b358a72a2db30089f3856475bb1f918d6d",
                    "network": "cosmoshub-4",
                    "moniker": "validator-foo",
                    "version": "v0.37.4"
                },
                "is_outbound": true,
                "connection_status": {
                    "Duration": "12345678901234"
                },
                "remote_ip": "192.0.2.1"
            }
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field                              | Type    | Description                                       |
| ---------------------------------- | ------- | ------------------------------------------------- |
| result.listening                   | boolean | Whether the node is accepting P2P connections     |
| result.n\_peers                    | string  | Total connected peer count (decimal string)       |
| result.peers                       | array   | Per-peer connection records                       |
| result.peers\[].node\_info.moniker | string  | Peer's node name                                  |
| result.peers\[].is\_outbound       | boolean | Whether the connection is outbound from this node |
| result.peers\[].remote\_ip         | string  | Remote peer IP address                            |

## Use Cases

* Network topology inspection and analytics
* Detecting isolated or poorly-connected nodes
* Validator peer-set monitoring

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="CosmJS" %}
```javascript
import { Tendermint37Client, HttpClient } from '@cosmjs/tendermint-rpc';

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "net_info", "params": []});
console.log(result);
```
{% endtab %}

{% tab title="cosmpy / requests" %}
```python
# cosmpy wraps Cosmos REST / gRPC. For raw CometBFT RPC access, use requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'net_info',
    'params': []
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
