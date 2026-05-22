---
description: >-
  Example code for the chain_getHeader JSON-RPC method. Complete guide on how to
  use chain_getHeader JSON-RPC in GetBlock Web3 documentation.
---

# chain\_getHeader - Midnight

This method returns the header of a block by its hash. Headers contain parent hash, number, state root, extrinsics root, and digest.

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| hash      | string | No       | Block hash to query. Defaults to the current best block |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "chain_getHeader",
    "params": [
        "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "chain_getHeader",
    "params": [
        "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"
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
    "method": "chain_getHeader",
    "params": [
        "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"
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

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "chain_getHeader",
        "params": [
                "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"
        ],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "parentHash": "0x86e4c08\u2026",
        "number": "0xef727",
        "stateRoot": "0x9e7c5e\u2026",
        "extrinsicsRoot": "0x56e81f\u2026",
        "digest": {
            "logs": []
        }
    }
}
```

## Response Parameters

| Field                 | Type   | Description                      |
| --------------------- | ------ | -------------------------------- |
| result.parentHash     | string | Hash of the parent block         |
| result.number         | string | Block number (hex)               |
| result.stateRoot      | string | State trie root after this block |
| result.extrinsicsRoot | string | Extrinsics trie root             |
| result.digest         | object | Block digest with consensus logs |

## Use Cases

* Light client header walks
* Indexers that need only metadata
* Verifying parent-chain relationships

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.chain.getHeader("0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95");
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('chain_getHeader', ["0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"]);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('chain_getHeader', ["0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"])
print(result)
```
{% endtab %}
{% endtabs %}
