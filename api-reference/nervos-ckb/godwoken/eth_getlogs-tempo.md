# eth\_getlogs tempo

Returns logs matching a given filter. Filters specify block range, address, and indexed topics — efficient retrieval of contract events like TIP-20 transfers.

## Parameters

| Parameter    | Type   | Required | Description                                                                                                 |
| ------------ | ------ | -------- | ----------------------------------------------------------------------------------------------------------- |
| filterObject | object | Yes      | Filter with optional `fromBlock`, `toBlock`, `address` (single or array), `topics` (array of topic filters) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x9c3b",
            "toBlock": "latest",
            "address": "0x4242424242424242424242424242424242424242",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x9c3b",
            "toBlock": "latest",
            "address": "0x4242424242424242424242424242424242424242",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x9c3b",
            "toBlock": "latest",
            "address": "0x4242424242424242424242424242424242424242",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
        "method": "eth_getLogs",
        "params": [
                {
                        "fromBlock": "0x9c3b",
                        "toBlock": "latest",
                        "address": "0x4242424242424242424242424242424242424242",
                        "topics": [
                                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
                        ]
                }
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
    "result": [
        {
            "address": "0x4242424242424242424242424242424242424242",
            "blockHash": "0x6baa8fa86b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d",
            "blockNumber": "0x9c40",
            "data": "0x0000000000000000000000000000000000000000000000000000000005f5e100",
            "logIndex": "0x0",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd56245",
                "0x0000000000000000000000009e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2"
            ],
            "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
            "transactionIndex": "0x0"
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field             | Type            | Description                                                         |
| ----------------- | --------------- | ------------------------------------------------------------------- |
| result            | array           | Array of log objects matching the filter                            |
| result\[].address | string          | Contract address that emitted the log                               |
| result\[].topics  | array of string | Indexed event topics; topic 0 is typically the event signature hash |
| result\[].data    | string          | Non-indexed event data (ABI-encoded)                                |

## Use Cases

* Indexing TIP-20 stablecoin transfers on Tempo
* Tracking Tempo stablecoin DEX swap events
* Building payment activity feeds for wallets and merchant dashboards
* Backfilling historical payment activity

## Error Handling

| Status Code | Error Message     | Cause                                                                               |
| ----------- | ----------------- | ----------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                                 |
| -32602      | Invalid params    | Request parameters are missing or malformed                                         |
| -32601      | Method not found  | Method does not exist or is not enabled on this node                                |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                   |
| -32005      | Limit exceeded    | Block range or returned result set exceeds the provider's limit; narrow your filter |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_getLogs', [{"fromBlock": "0x9c3b", "toBlock": "latest", "address": "0x4242424242424242424242424242424242424242", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}]);
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

const result = await client.request({ method: 'eth_getLogs', params: [{"fromBlock": "0x9c3b", "toBlock": "latest", "address": "0x4242424242424242424242424242424242424242", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}] });
console.log(result);
```
{% endtab %}
{% endtabs %}
