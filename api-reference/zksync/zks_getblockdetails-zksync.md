---
description: >-
  Example code for the zks_getBlockDetails JSON-RPC method. Сomplete guide on
  how to use zks_getBlockDetails JSON-RPC in GetBlock.io Web3 documentation.
---

# zks\_getBlockDetails - zkSync

Returns zkSync-specific details for an L2 block: status (verified/proven/committed/executed), corresponding L1 batch, operator address, protocol version, and gas prices.

## Parameters

| Parameter   | Type    | Required | Description                  |
| ----------- | ------- | -------- | ---------------------------- |
| blockNumber | integer | Yes      | The L2 block number to query |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getBlockDetails",
    "params": [
        2247607
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
    "method": "zks_getBlockDetails",
    "params": [
        2247607
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
    "method": "zks_getBlockDetails",
    "params": [
        2247607
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
        "method": "zks_getBlockDetails",
        "params": [
                2247607
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
    "id": "getblock.io",
    "result": {
        "number": 2247607,
        "l1BatchNumber": 468355,
        "timestamp": 1679815038,
        "l1TxCount": 0,
        "l2TxCount": 20,
        "rootHash": "0xf1adac176fc939313eea4b72055db0622a10bbd9b7a83097286e84e471d2e7df",
        "status": "verified",
        "commitTxHash": "0xd045e3698f018cb233c3817eb53a41a4c5b28784ffe659da246aa33bda34350c",
        "committedAt": "2023-03-26T07:21:21.046817Z",
        "proveTxHash": "0x1591e9b16ff6eb029cc865614094b2e6dd872c8be40b15cc56164941ed723a1a",
        "provenAt": "2023-03-26T19:48:35.200565Z",
        "executeTxHash": "0xbb66aa75f437bb4255cf751badfc6b142e8d4d3a4e531c7b2e737a22870ff19e",
        "executedAt": "2023-03-27T07:44:52.187764Z",
        "l1GasPrice": 20690385511,
        "l2FairGasPrice": 250000000,
        "baseSystemContractsHashes": {
            "bootloader": "0x010007793a328ef16cc7086708f7f3292ff9b5eed9e7e539c184228f461bf4ef",
            "default_aa": "0x0100067d861e2f5717a12c3e869cfb657793b86bbb0caa05cc1421f16c5217bc"
        },
        "operatorAddress": "0xfeee860e7aae671124e9a4e61139f3a5085dfeee",
        "protocolVersion": "Version5"
    }
}
```
{% endcode %}

## Response Parameters

| Field                  | Type    | Description                                                  |
| ---------------------- | ------- | ------------------------------------------------------------ |
| result.number          | integer | L2 block number                                              |
| result.l1BatchNumber   | integer | Corresponding L1 batch number                                |
| result.timestamp       | integer | Unix timestamp when the block was committed                  |
| result.status          | string  | Block status — `committed`, `proven`, `executed`, `verified` |
| result.commitTxHash    | string  | L1 transaction hash of the commit operation                  |
| result.proveTxHash     | string  | L1 transaction hash of the proof submission                  |
| result.executeTxHash   | string  | L1 transaction hash of the execution                         |
| result.operatorAddress | string  | Address of the operator who committed the block              |
| result.protocolVersion | string  | zkSync protocol version this block was committed under       |

## Use Cases

* Explorers showing block-to-L1-batch mapping
* Auditing block finality on Ethereum mainnet
* Computing per-block settlement latency

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('zks_getBlockDetails', [2247607]);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'zks_getBlockDetails'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('zks_getBlockDetails', [2247607])
print(result)
```
{% endtab %}
{% endtabs %}
