---
description: >-
  Example code for the eth_getBlockByNumber Flashblocks method. Complete guide
  on how to use eth_getBlockByNumber  Flashblocks in GetBlock Web3
  documentation.
---

# eth\_getBlockByNumber - Flashblocks

This returns the current in-progress Flashblock when called with `"pending"` — including every transaction that has landed in Flashblocks since the last block sealed. Pass `true` as the second parameter for full transaction objects; `false` for hashes only. When called with `"latest"`, returns the last sealed block as usual. Preconfirmed Flashblocks are distinguishable from finalized blocks by their zero `stateRoot` and by the absence of a final block hash.

## Parameters

| Parameter        | Type     | Required | Description                                                  |
| ---------------- | -------- | -------- | ------------------------------------------------------------ |
| blockNumber      | QUANTITY | TAG      | Yes                                                          |
| fullTransactions | boolean  | Yes      | `true` for full transaction objects, `false` for hashes only |

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
        "pending",
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
        "pending",
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
        "pending",
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
                "pending",
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
        "number": "0x16e3600",
        "hash": null,
        "parentHash": "0x47edbdc8da5cd1b3127ea8f1ce5da6739fa39e7e6d2eb1c9b50f1c83de0a98b4",
        "timestamp": "0x68f3b85f",
        "miner": "0x4200000000000000000000000000000000000011",
        "gasLimit": "0x3938700",
        "gasUsed": "0x1c2b40",
        "baseFeePerGas": "0x2540be400",
        "stateRoot": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "transactions": [
            "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
        ],
        "size": "0x42a"
    }
}
```
{% endcode %}

## Response Parameters

| Field               | Type     | Description                                                                         |
| ------------------- | -------- | ----------------------------------------------------------------------------------- |
| result.number       | QUANTITY | In-progress block number (the same as the next `latest` will be)                    |
| result.hash         | DATA     | `null` for Flashblocks (block not yet sealed) — a real hash appears only after seal |
| result.parentHash   | DATA     | Parent block hash (last sealed block)                                               |
| result.stateRoot    | DATA     | Zero hash on Flashblocks (state not yet committed); real root when block seals      |
| result.transactions | array    | Every transaction preconfirmed into Flashblocks so far in this in-progress block    |
| result.timestamp    | QUANTITY | Timestamp of the in-progress block                                                  |

## Use Cases

* Fast block scanning — read the current preconfirmed state up to 1.8 seconds before the block seals
* Building real-time DeFi dashboards that show transactions as they land
* MEV analysis — inspecting sequencer-ordered transactions before finalization
* Detecting the current Flashblock index to time downstream operations

## Error Handling

| Status Code | Error Message         | Cause                                                                          |
| ----------- | --------------------- | ------------------------------------------------------------------------------ |
| 403         | Forbidden             | Missing or invalid `<ACCESS-TOKEN>`                                            |
| -32602      | Invalid params        | Request parameters are missing or malformed                                    |
| -32603      | Internal error        | Server-side error while processing the request                                 |
| 429         | Too Many Requests     | Rate limit exceeded for your plan                                              |
| -32000      | Sequencer unavailable | Flashblocks stream is temporarily down — response falls back to `latest` state |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call — 'pending' returns Flashblocks-preconfirmed state:
const result = await provider.send('eth_getBlockByNumber', ["pending", false]);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider that accept 'pending':
// provider.getBalance(addr, 'pending'), provider.getTransactionCount(addr, 'pending'), etc.
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// viem's read methods accept blockTag: 'pending' for Flashblocks-preconfirmed state:
const result = await client.request({
    method: 'eth_getBlockByNumber',
    params: ["pending", false]
});
console.log(result);

// Typed wrappers also accept blockTag: 'pending':
// client.getBalance({ address, blockTag: 'pending' })
// client.getBlock({ blockTag: 'pending' })
```
{% endtab %}
{% endtabs %}
