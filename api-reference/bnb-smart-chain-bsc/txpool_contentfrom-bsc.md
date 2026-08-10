---
description: >-
  Example code for the txpool_contentFrom JSON RPC method. Complete guide on how
  to use txpool_contentFrom JSON RPC in GetBlock Web3 documentation.
---

# txpool\_contentFrom - BSC

This method returns the pending and queued transactions in the transaction pool that originate from a single sender address on the BNB Smart Chain. It is the scoped counterpart to [txpool\_content](txpool_content-bsc.md), and returns a small response even when the mempool is busy.

{% hint style="info" %}
Because the sender is already fixed by the parameter, the `pending` and `queued` objects are keyed **directly by nonce**, without the extra address level that `txpool_content` returns.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                              |
| --------- | ------ | -------- | ---------------------------------------- |
| address   | string | Yes      | The sender address to filter the pool by |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "txpool_contentFrom",
    "params": ["0x000000000ce4f22a3cd50cc9a1ffe7f2417c5966"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'txpool_contentFrom',
    params: ['0x000000000ce4f22a3cd50cc9a1ffe7f2417c5966'],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const { pending, queued } = response.data.result;
    console.log('Pending nonces:', Object.keys(pending));
    console.log('Queued nonces:', Object.keys(queued));
})
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "txpool_contentFrom",
    "params": ["0x000000000ce4f22a3cd50cc9a1ffe7f2417c5966"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
result = response.json()["result"]
print(f"Pending nonces: {list(result['pending'])}")
print(f"Queued nonces: {list(result['queued'])}")
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "txpool_contentFrom",
        "params": ["0x000000000ce4f22a3cd50cc9a1ffe7f2417c5966"],
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
        "pending": {
            "3": {
                "blockHash": null,
                "blockNumber": null,
                "from": "0x000000000ce4f22a3cd50cc9a1ffe7f2417c5966",
                "gas": "0x5208",
                "gasPrice": "0x1693284",
                "hash": "0x0f1a6a2c8f1a3a3b8f5f2c0a9e5b1d7c4a6e8b0d2f4a6c8e0b2d4f6a8c0e2b4d",
                "input": "0x",
                "nonce": "0x3",
                "to": "0xdec03919846f1b929ead493a927d3bda27a33d2e",
                "transactionIndex": null,
                "value": "0x2ee0",
                "type": "0x0",
                "chainId": "0x38",
                "v": "0x93",
                "r": "0x3c9f1a7d5e2b8c4a6f0d9e3b7a5c1f8d2e6b4a0c9d7f3e1b5a8c2d6f4e0b9a7c",
                "s": "0x1b8d4f2a6c0e9b7d5f3a1c8e6b4d2f0a9c7e5b3d1f8a6c4e2b0d9f7a5c3e1b8d"
            }
        },
        "queued": {}
    }
}
```

When the address has nothing in the pool, both objects are returned empty:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "pending": {},
        "queued": {}
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                  |
| --------- | ------ | ---------------------------------------------------------------------------- |
| pending   | object | Transactions from the address eligible for inclusion, keyed by decimal nonce |
| queued    | object | Transactions from the address not yet processable, keyed by decimal nonce    |

Each transaction object carries the same fields as [txpool\_content](txpool_content-bsc.md): `from`, `to`, `gas`, `gasPrice`, `nonce`, `value`, `input`, `type`, `chainId`, and the `v` / `r` / `s` signature components, with `blockHash`, `blockNumber`, and `transactionIndex` set to `null` while the transaction is still pooled.

## Use Cases

* Check whether a specific transaction you submitted is still pending
* Detect a nonce gap that is blocking an account's queued transactions
* Monitor a hot wallet's outstanding transactions without pulling the whole mempool
* Confirm a replacement transaction has displaced the original at the same nonce

## Error Handling

| Error Code | Description                                  |
| ---------- | -------------------------------------------- |
| -32601     | Method not supported                         |
| -32602     | Invalid params, e.g. the address was omitted |
| -32603     | Internal error                               |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const address = '0x000000000ce4f22a3cd50cc9a1ffe7f2417c5966';
const content = await provider.send('txpool_contentFrom', [address]);
console.log('Pending nonces:', Object.keys(content.pending));
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const content = await client.request({
    method: 'txpool_contentFrom',
    params: ['0x000000000ce4f22a3cd50cc9a1ffe7f2417c5966']
});
console.log('Pending nonces:', Object.keys(content.pending));
```
{% endtab %}
{% endtabs %}
