---
description: >-
  Example code for the eth_getChainConfig JSON-RPC method. Complete guide on how
  to use eth_getChainConfig JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getChainConfig - AVAX

Returns the C-Chain runtime configuration — including all activated hard forks, fee config, and validator info. Avalanche-specific extension.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getChainConfig",
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
    "method": "eth_getChainConfig",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getChainConfig",
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
        "method": "eth_getChainConfig",
        "params": [],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc")
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
        "chainId": 43114,
        "homesteadBlock": 0,
        "eip150Block": 0,
        "byzantiumBlock": 0,
        "constantinopleBlock": 0,
        "petersburgBlock": 0,
        "istanbulBlock": 0,
        "muirGlacierBlock": 0,
        "apricotPhase1BlockTimestamp": 1612230000,
        "apricotPhase2BlockTimestamp": 1620320400,
        "apricotPhase3BlockTimestamp": 1629839640,
        "apricotPhase4BlockTimestamp": 1633042200,
        "apricotPhase5BlockTimestamp": 1638203000,
        "banffBlockTimestamp": 1669144680,
        "cortinaBlockTimestamp": 1684339200,
        "durangoBlockTimestamp": 1712863200,
        "etnaTimestamp": 1727712000,
        "feeConfig": {
            "gasLimit": 15000000,
            "minBaseFee": 25000000000,
            "targetGas": 15000000,
            "baseFeeChangeDenominator": 36,
            "minBlockGasCost": 0,
            "maxBlockGasCost": 1000000,
            "targetBlockRate": 2,
            "blockGasCostStep": 200000
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                            | Type    | Description                                                                           |
| -------------------------------- | ------- | ------------------------------------------------------------------------------------- |
| result.chainId                   | integer | Numeric chain ID (43114 mainnet, 43113 Fuji)                                          |
| result.feeConfig                 | object  | Dynamic fee configuration parameters                                                  |
| result.feeConfig.minBaseFee      | integer | Minimum base fee in wei                                                               |
| result.feeConfig.gasLimit        | integer | Block gas limit                                                                       |
| result.feeConfig.targetBlockRate | integer | Target block production rate in seconds                                               |
| result.\*BlockTimestamp          | integer | Activation timestamps for Avalanche upgrades (Apricot, Banff, Cortina, Durango, Etna) |

## Use Cases

* Detecting which protocol upgrades are active
* Adaptive fee oracles that respect the network's dynamic fee config
* Tooling that needs to know the block gas limit and target rate

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_getChainConfig', []);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { avalanche } from 'viem/chains';

const client = createPublicClient({
    chain: avalanche,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc')
});

const result = await client.request({ method: 'eth_getChainConfig', params: [] });
console.log(result);
```
{% endtab %}
{% endtabs %}
