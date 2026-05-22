---
description: >-
  Example code for the state_getReadProof JSON-RPC method. Complete guide on how
  to use state_getReadProof JSON-RPC in GetBlock Web3 documentation.
---

# state\_getReadProof - Midnight

This method returns a Merkle proof of inclusion for one or more storage keys. Light clients use these proofs to verify storage values without trusting the RPC provider.

## Parameters

| Parameter | Type            | Required | Description                                                |
| --------- | --------------- | -------- | ---------------------------------------------------------- |
| keys      | array of string | Yes      | Array of hex-encoded storage keys                          |
| hash      | string          | No       | Block hash to query at. Defaults to the current best block |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "state_getReadProof",
    "params": [
        [
            "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
        ]
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
    "method": "state_getReadProof",
    "params": [
        [
            "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
        ]
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
    "method": "state_getReadProof",
    "params": [
        [
            "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
        ]
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
        "method": "state_getReadProof",
        "params": [
                [
                        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
                ]
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
        "at": "0x6cb7a43a817c03d906085d1e69e74a8cf2d7036b65948f9940ed816b0993288d",
        "proof": [
            "0x804044804a5d4b2f9f77e6b75525cf500ce72c98653e48a56b5d8422c49b24d3e2ff8f9880aa3ddb591ac2153359d72e96e039a403a1edfd7241894ff97e8d1515712428f280cfdfd98e29bc23304a008993a12f8bbcf13d4545a9766c64424c98e01c21ae00",
            "0x807dfd80cb9ea7ee2f57d9d052ad7f99e16bfdce6f43469e0e2733d6f3549543d18aa959809930909d326640377e3b2584dc68a4cdcb049a1afcfe6fbdbef6892866c53db180a0a32c427264bc438e4d80c87cf8d1e265beb6d74090c6c5d38727944cd5c32980799dcab613fc6ccef76f7f246092e7b35e047d4a697061059757785a16b107dc80339888a68e3a4969592e3c951fef11e59ecdddea3cc6c193a0f242b2388e3f0f807ce16a473d4e4189d5273afa706fb34dde6caf91e61ddb1481ee5d71b8297f438083d0ce9089c3d54b4ad5620e29bdc86d8fc9ccc98209e489561fcd2d2fe2957a80d191a1aebcccbd85728f7b0f1f21d6a7188a65bffed02492df4d391ad2e276a780b1f8b62786dc88681fe2e33e1062b484948d4595c4cd69f118b01a5332bb8fe8808d97d59bfa6701a59498deb971213d5476d2e28ded75042183374dde740914d380f9301ba37a1430856c624d3e305a0a09b900845ddf916cb024988b10b077b55780a30d451367277433f530a2bfeac045d51479376d2cec4f103ed0fe9c6277199780d8397b1086d98e15b7adeb21cbc5eff58d511f31fd44c810dca749cf781b3b7c",
            "0x9eaa394eea5630e07c48ae0c9558cef7398f80efc1f0290ab8face3fc1be51bc48cea66f696276832640ac79cee6e5c57ce1f4806bf9bd7f852331e2a1877ec5219f0addb47a4cea6b222b93c00266f4aa8d0411505f0e7b9012096b41c4eb3aaf947f6ea4290800004c5f0684a022a34dd8bfa2baaf44f172b7100401808b9556a81a27faac59e4970a619043c3140578085c7196052139f7500b66037280be6bc40b7907235c2f6f8c679815127275cc4731e90e62f93b361cc2d631f143802459c92895bb867913b5422e873b37a4014032ef68468f690a93a9bc1dff941f803f57e42cca53a2c53ff625db9b6fe62a0180b86547d1f0ede0c509614c76f5707c5f09cce9c888469bb1a0dceaa129672ef834c2570100206d69646e69676874",
            "0x9f099d880ec681799c0cf30e8886371da94a1a807dc6505220b970a325b0cb6237a89ed72fe89f3bbdf5a73e7f4e59a6fba2122080ce196a4c4def7a65a314edd59805d8aa089da2c23184e61c63fdb4c30df6fe3f806d6a6df9514b0d0a94b0b0af03680b330fb634894bdabb0764224c4220e5a9a080641795428037f41075c9e9b6186339644646541893a1bca067281e71d72b2b9780437cc59e93d9b578969444956ca2dc049cef1a7aeb0501747a7a031cd12adfa680ed3d24a67fffac9a4368290654981f1491a6f63b2d26f960759995261a60b9e5"
        ]
    },
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field        | Type            | Description                                     |
| ------------ | --------------- | ----------------------------------------------- |
| result.at    | string          | Block hash at which the proof was generated     |
| result.proof | array of string | Hex-encoded Merkle trie nodes forming the proof |

## Use Cases

* Light client state verification
* Cross-chain bridges that prove Midnight state to other chains
* Trust-minimized state queries

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
const result = await api.rpc.state.getReadProof(["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"]);
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('state_getReadProof', [["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"]]);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('state_getReadProof', [["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"]])
print(result)
```
{% endtab %}
{% endtabs %}
