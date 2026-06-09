# eth\_getblockbynumber tempo

Returns information about a block by block number. Pass `"latest"`, `"earliest"`, `"pending"`, `"safe"`, or `"finalized"` as a block tag, or a hex-encoded block number. On Tempo, `"finalized"` reflects deterministic single-slot finality — there are no re-orgs, so finalized tracks very close to `"latest"`.

## Parameters

| Parameter        | Type    | Required | Description                                                                            |
| ---------------- | ------- | -------- | -------------------------------------------------------------------------------------- |
| blockParameter   | string  | Yes      | Block number in hex, or `"latest"`, `"earliest"`, `"pending"`, `"safe"`, `"finalized"` |
| fullTransactions | boolean | Yes      | If `true`, returns full transaction objects; if `false`, only hashes                   |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
        false
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
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
        false
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
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
        false
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
        "method": "eth_getBlockByNumber",
        "params": [
                "latest",
                false
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
        "number": "0x9c40",
        "hash": "0x6baa8fa86b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d",
        "parentHash": "0x3a2e9d4b1c5f8a7e6d4b2c1f9a3e5d8b7c2f4a1d6e9b3c5f8a2d4e7b1c9f3a6e",
        "nonce": "0x0000000000000000",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "logsBloom": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "transactionsRoot": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        "stateRoot": "0xf1adac176fc939313eea4b72055db0622a10bbd9b7a83097286e84e471d2e7df",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "miner": "0x0000000000000000000000000000000000000000",
        "difficulty": "0x0",
        "totalDifficulty": "0x0",
        "extraData": "0x",
        "size": "0x4d2",
        "gasLimit": "0x1c9c380",
        "gasUsed": "0x12345",
        "baseFeePerGas": "0x0",
        "timestamp": "0x67891f24",
        "transactions": [
            "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
        ],
        "uncles": []
    }
}
```
{% endcode %}

## Response Parameters

| Field                | Type   | Description                                                                              |
| -------------------- | ------ | ---------------------------------------------------------------------------------------- |
| result.number        | string | Block number (hex)                                                                       |
| result.hash          | string | Block hash (32 bytes hex)                                                                |
| result.parentHash    | string | Hash of the parent block                                                                 |
| result.timestamp     | string | Unix timestamp of the block (hex)                                                        |
| result.gasUsed       | string | Total gas used by transactions in this block (hex)                                       |
| result.baseFeePerGas | string | Base fee per gas (hex). On Tempo this is denominated in TIP-20 fee units, not native ETH |
| result.transactions  | array  | Array of transaction hashes, or full transaction objects if `fullTransactions=true`      |

## Use Cases

* Block-by-block indexing of Tempo transactions
* Building payment-focused explorers
* Computing per-block statistics

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_getBlockByNumber', ["latest", false]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, defineChain } from 'viem';

// Tempo is not yet in viem/chains as a built-in — define it inline.
const tempo = defineChain({
    id: 4217,
    name: 'Tempo',
    network: 'tempo',
    // Tempo has no native gas token. The `nativeCurrency` field is required by viem's
    // type system; values shown here are a no-op placeholder — actual fees are paid in TIP-20.
    nativeCurrency: { name: 'USD Placeholder', symbol: 'USD', decimals: 18 },
    rpcUrls: {
        default: { http: ['https://go.getblock.io/<ACCESS-TOKEN>/'] }
    },
    blockExplorers: { default: { name: 'Tempo Explorer', url: 'https://explore.tempo.xyz' } }
});

const client = createPublicClient({
    chain: tempo,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_getBlockByNumber', params: ["latest", false] });
console.log(result);
```
{% endtab %}
{% endtabs %}
