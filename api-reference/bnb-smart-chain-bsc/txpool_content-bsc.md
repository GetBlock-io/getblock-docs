---
description: >-
  Example code for the txpool_content JSON RPC method. Complete guide on how to
  use txpool_content JSON RPC in GetBlock Web3 documentation.
---

# txpool\_content - BSC

This method returns all pending and queued transactions currently held in the transaction pool on the BNB Smart Chain, grouped first by sender address and then by nonce.

{% hint style="info" %}
The BSC mempool routinely holds thousands of transactions across hundreds of senders, so a full `txpool_content` response can be several megabytes. Use txpool\_status for counts only, [txpool\_inspect](txpool_inspect-bsc.md) for a compact text summary, or [txpool\_contentFrom](txpool_contentfrom-bsc.md) to scope the result to a single sender.
{% endhint %}

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "txpool_content",
    "params": [],
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
    method: 'txpool_content',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const { pending, queued } = response.data.result;
    console.log('Pending senders:', Object.keys(pending).length);
    console.log('Queued senders:', Object.keys(queued).length);
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
    "method": "txpool_content",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
result = response.json()["result"]
print(f"Pending senders: {len(result['pending'])}")
print(f"Queued senders: {len(result['queued'])}")
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
        "method": "txpool_content",
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "pending": {
            "0x0000000000dd326d3e6d23a88571182d01d79940": {
                "12863": {
                    "blockHash": null,
                    "blockNumber": null,
                    "from": "0x0000000000dd326d3e6d23a88571182d01d79940",
                    "gas": "0x186a0",
                    "gasPrice": "0x2faf468",
                    "maxFeePerGas": "0x2faf468",
                    "maxPriorityFeePerGas": "0x2faf468",
                    "hash": "0x85d656bceb127e8e7bd18cf9565c7e79f0f7e289cdcd19e0c22656aef6927242",
                    "input": "0xa9059cbb0000000000000000000000005b50ea5757b87736e0c84d4a4a484f42b7bc45c20000000000000000000000000000000000000000000000000022b9e9c8634e8e",
                    "nonce": "0x323f",
                    "to": "0x55d398326f99059ff775485246999027b3197955",
                    "transactionIndex": null,
                    "value": "0x0",
                    "type": "0x2",
                    "accessList": [],
                    "chainId": "0x38",
                    "v": "0x0",
                    "r": "0x9803073d9c4275c35efdfb55334a59d2bcb39adda0119bdfc40a41e36503e262",
                    "s": "0x7bfa23a20c1cecef2208d86376bb18bcae4c5854b2e8a261a26b9b23300c3c5f",
                    "yParity": "0x0"
                }
            }
        },
        "queued": {
            "0x000000000ce4f22a3cd50cc9a1ffe7f2417c5966": {
                "7": {
                    "blockHash": null,
                    "blockNumber": null,
                    "from": "0x000000000ce4f22a3cd50cc9a1ffe7f2417c5966",
                    "gas": "0x5208",
                    "gasPrice": "0x1693284",
                    "hash": "0x0f1a6a2c8f1a3a3b8f5f2c0a9e5b1d7c4a6e8b0d2f4a6c8e0b2d4f6a8c0e2b4d",
                    "input": "0x",
                    "nonce": "0x7",
                    "to": "0xdec03919846f1b929ead493a927d3bda27a33d2e",
                    "transactionIndex": null,
                    "value": "0x2ee0",
                    "type": "0x0",
                    "chainId": "0x38",
                    "v": "0x93",
                    "r": "0x3c9f1a7d5e2b8c4a6f0d9e3b7a5c1f8d2e6b4a0c9d7f3e1b5a8c2d6f4e0b9a7c",
                    "s": "0x1b8d4f2a6c0e9b7d5f3a1c8e6b4d2f0a9c7e5b3d1f8a6c4e2b0d9f7a5c3e1b8d"
                }
            }
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                         |
| --------- | ------ | ----------------------------------------------------------------------------------- |
| pending   | object | Transactions eligible for inclusion, keyed by sender address, then by decimal nonce |
| queued    | object | Transactions not yet processable (usually a nonce gap), with the same nesting       |

Each transaction object carries the standard BSC transaction fields:

| Field                                      | Type   | Description                                                |
| ------------------------------------------ | ------ | ---------------------------------------------------------- |
| blockHash / blockNumber / transactionIndex | null   | Always `null` while a transaction is still in the pool     |
| from / to                                  | string | Sender and recipient. `to` is `null` for contract creation |
| gas / gasPrice                             | string | Gas limit and gas price, hex-encoded                       |
| maxFeePerGas / maxPriorityFeePerGas        | string | EIP-1559 fee fields, present on `type` `0x2` transactions  |
| nonce                                      | string | Sender nonce, hex-encoded                                  |
| value                                      | string | BNB transferred, in wei, hex-encoded                       |
| input                                      | string | Encoded call data                                          |
| type                                       | string | Transaction type: `0x0` legacy, `0x2` EIP-1559             |
| accessList                                 | array  | EIP-2930 access list, present on typed transactions        |
| chainId                                    | string | `0x38` for BSC mainnet                                     |
| v / r / s / yParity                        | string | Signature components                                       |

## Use Cases

* Inspect the full mempool before submitting a competing transaction
* Detect stuck transactions and nonce gaps for a set of accounts
* Build MEV and arbitrage tooling that reacts to unconfirmed transactions
* Debug why a submitted transaction is not being mined

## Error Handling

| Error Code | Description          |
| ---------- | -------------------- |
| -32601     | Method not supported |
| -32603     | Internal error       |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const content = await provider.send('txpool_content', []);
console.log('Pending senders:', Object.keys(content.pending).length);
console.log('Queued senders:', Object.keys(content.queued).length);
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

const content = await client.request({ method: 'txpool_content' });
console.log('Pending senders:', Object.keys(content.pending).length);
```
{% endtab %}
{% endtabs %}
