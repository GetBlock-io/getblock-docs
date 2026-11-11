---
description: >-
  Example code for the txpool_inspect JSON RPC method. Complete guide on how to
  use txpool_inspect JSON RPC in GetBlock Web3 documentation.
---

# txpool\_inspect - BSC

This method returns a compact, human-readable summary of the pending and queued transactions in the transaction pool on the BNB Smart Chain. It has the same sender-and-nonce nesting as [txpool\_content](txpool_content-bsc.md), but each transaction is reduced to a single summary string instead of a full object, which makes the response far smaller.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "txpool_inspect",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'txpool_inspect',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const { pending } = response.data.result;
    for (const [sender, byNonce] of Object.entries(pending)) {
        for (const [nonce, summary] of Object.entries(byNonce)) {
            console.log(`${sender} #${nonce} -> ${summary}`);
        }
    }
})
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "txpool_inspect",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
pending = response.json()["result"]["pending"]
for sender, by_nonce in pending.items():
    for nonce, summary in by_nonce.items():
        print(f"{sender} #{nonce} -> {summary}")
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
        "method": "txpool_inspect",
        "params": [],
        "id": "getblock.io"
    });

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
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
        "pending": {
            "0x000000005f49971f36545bc6b07b8fb92c3999b1": {
                "1263681": "0x000000002BCD37f2bB3B5ACab9Ff5E247556f592: 0 wei + 1208900 gas × 50000000 wei"
            },
            "0x00000000acd597fa0cbeec9f9a094252d5236082": {
                "1020711": "0x000000002BCD37f2bB3B5ACab9Ff5E247556f592: 0 wei + 1208900 gas × 50000000 wei"
            }
        },
        "queued": {
            "0x000000000ce4f22a3cd50cc9a1ffe7f2417c5966": {
                "7": "0xdec03919846f1b929ead493a927d3bda27a33d2e: 12000 wei + 21000 gas × 23675012 wei"
            }
        }
    }
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                                                 |
| --------- | ------ | --------------------------------------------------------------------------- |
| pending   | object | Transactions eligible for inclusion, keyed by sender address, then by nonce |
| queued    | object | Transactions not yet processable, with the same nesting                     |

Each leaf value is a string in the form `<to>: <value> wei + <gas> gas × <gasPrice> wei`, where all numbers are decimal rather than hex-encoded.

| Segment    | Description                  |
| ---------- | ---------------------------- |
| `to`       | Recipient address            |
| `value`    | BNB transferred, in wei      |
| `gas`      | Gas limit of the transaction |
| `gasPrice` | Gas price offered, in wei    |

## Use Cases

* Survey mempool contents without transferring megabytes of transaction data
* Spot senders offering unusually high gas prices
* Identify nonce gaps that leave transactions stuck in the queued set
* Feed lightweight mempool dashboards and alerting

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

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const inspect = await provider.send('txpool_inspect', []);
console.log(Object.keys(inspect.pending).length, 'senders with pending transactions');
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const inspect = await client.request({ method: 'txpool_inspect' });
console.log(Object.keys(inspect.pending).length, 'senders with pending transactions');
```
{% endtab %}
{% endtabs %}
