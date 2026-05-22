---
description: >-
  Example code for the chain_getBlock JSON-RPC method. Complete guide on how to
  use chain_getBlock JSON-RPC in GetBlock Web3 documentation.
---

# chain\_getBlock - Midnight

This method returns full block data for a given block hash, including header and extrinsics. For a fully decoded JSON view, use `midnight_jsonBlock` instead.

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
    "method": "chain_getBlock",
    "params": [
        "0x0572f7f97b772f177a1749990259ed9715d09e966c9ab8ce837c181833ecb6b8"
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
    "method": "chain_getBlock",
    "params": [
        "0x0572f7f97b772f177a1749990259ed9715d09e966c9ab8ce837c181833ecb6b8"
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
    "method": "chain_getBlock",
    "params": [
        "0x0572f7f97b772f177a1749990259ed9715d09e966c9ab8ce837c181833ecb6b8"
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
        "method": "chain_getBlock",
        "params": [
                "0x0572f7f97b772f177a1749990259ed9715d09e966c9ab8ce837c181833ecb6b8"
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "result": {
        "block": {
            "header": {
                "parentHash": "0x1ff9528c2504ee4c5d6d808f236350f132349659247b83b1fe3aef958991e6b0",
                "number": "0xd3526",
                "stateRoot": "0xadd725f6d76b73d9074e631832146f15c3b443088b7bc80b7d0596c06e00f1fc",
                "extrinsicsRoot": "0xe14537dd0bc3782f7a149655feb2d24e592f60b1b4020718dfe2d688c763fc8d",
                "digest": {
                    "logs": [
                        "0x06617572612088ffac1100000000",
                        "0x066d637368806675af47c3d29466bcf23c980bb5416d009d242c254095b5c1c909a27db40e10",
                        "0x044d4e535610f0550000",
                        "0x04424545468403c74ace869374056bf0890407711462b9480df96f541d913a85e83020cd6ba0fb",
                        "0x05617572610101f00af1bd31d24b4ebbd43631a08236896f70c7cd03633621680fbf253683946ad8f52a27cc35f91b578f8abecdade961af72828254f845c54dc89d7b46171187"
                    ]
                }
            },
            "extrinsics": [
                "0x280501000b8103a5469e01",
                "0xd0050d00006675af47c3d29466bcf23c980bb5416d009d242c254095b5c1c909a27db40e10122a4800080ca4439e01000001000000",
                "0xb505052d000ccccd6dbd01b95948f56bb84ad441f29608b12b3694a2a71ce4ba0fa8c07f7f4bf932cb4c0de84606b3da87214324887270f5fb0e04a6870dc7df5f2312165fdd275029f4812daa0ba8bf416aea14c62db1e4223ff427b81f50ccac611ef4e15cba8217811b9a9b2a7ec4a24a110418a38c2a9f0ae7127e040e2ef42425883ac46002962a5201ea7bbbe40dc8d8542ec148d3ff32d5bbd71b8585125fa41171f55c23cdadc3d0ad3692be927c6ed1b92d1f0483350cde2306334193be59122367e5a774769e59de84baacfd8e136fba8e18dbcd08333958ae4a79fa36f52c9e0f5fab7aac2d4c4446a290b44e2d2f53d3878c457a4b2383443ff5b30420aea92bfca65971fd0b76d21715529e4e8192be1dc6f2de5adbbf0b77adcc6883d562a4f5a535017eaedc6804c5e55b33f6aa16d4c6892575af371fd14e1e40a7c4675876e8f331e2e2466a28e950765fa7b42151bbc97e9ecd40f454d6dd0a24cf3e579c675f6552bd059c82"
            ]
        },
        "justifications": null
    },
    "id": "getblock.io"
}

```
{% endcode %}

## Response Parameters

| Field                   | Type            | Description                             |
| ----------------------- | --------------- | --------------------------------------- |
| result.block.header     | object          | Block header                            |
| result.block.extrinsics | array of string | Hex-encoded SCALE-serialized extrinsics |
| result.justifications   | array \| null   | Finality justifications, if available   |

## Use Cases

* Block-by-block indexing
* Replaying historical extrinsics
* Building explorers with custom decoders

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
const result = await api.rpc.chain.getBlock("0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95");
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('chain_getBlock', ["0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"]);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('chain_getBlock', ["0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"])
print(result)
```
{% endtab %}
{% endtabs %}
