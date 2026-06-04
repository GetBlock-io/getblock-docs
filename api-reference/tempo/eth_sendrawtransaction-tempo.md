---
description: >-
  Example code for the eth_sendRawTransaction JSON-RPC method. Complete guide on
  how to use eth_sendRawTransaction JSON-RPC in GetBlock Web3 documentation.
---

# eth\_sendRawTransaction - Tempo

Broadcasts a signed transaction. Accepts both standard EVM transaction types (`0x0` legacy, `0x1` EIP-2930, `0x2` EIP-1559, `0x7702` EIP-7702) **and Tempo Transactions (type `0x54`)** — a Tempo-specific transaction type used for payment flows with structured memo fields, fee-payer sponsorship, and direct routing to subblock proposers. Transactions targeting a specific subblock proposer are routed to the consensus layer when submitted to the matching validator node; other nodes reject them. The transaction `value` field is effectively ignored — fees and transfers are TIP-20-denominated, not native-token-denominated.

{% hint style="warning" %}
**MODIFIED FROM STANDARD EVM.** Tempo has no native gas token — this method's behavior differs from standard Ethereum nodes. See the description below for the Tempo-specific semantics.
{% endhint %}

## Parameters

| Parameter    | Type   | Required | Description                                                                            |
| ------------ | ------ | -------- | -------------------------------------------------------------------------------------- |
| signedTxData | string | Yes      | The signed transaction data (hex). Standard EVM types or Tempo Transaction type `0x54` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0x02f86c8210798504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
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
    "method": "eth_sendRawTransaction",
    "params": [
        "0x02f86c8210798504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
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
    "method": "eth_sendRawTransaction",
    "params": [
        "0x02f86c8210798504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
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
        "method": "eth_sendRawTransaction",
        "params": [
                "0x02f86c8210798504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
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
    "result": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                               |
| ------ | ------ | ------------------------------------------------------------------------- |
| result | string | Transaction hash, used to track inclusion via `eth_getTransactionReceipt` |

## Use Cases

* Submitting TIP-20 stablecoin transfers (the primary Tempo use case)
* Executing payment flows with attached memos
* Submitting Tempo Transactions (type `0x54`) for advanced payment features
* Deploying smart contracts on Tempo

## Error Handling

| Status Code | Error Message                  | Cause                                                                        |
| ----------- | ------------------------------ | ---------------------------------------------------------------------------- |
| 403         | Forbidden                      | Missing or invalid `<ACCESS-TOKEN>`                                          |
| -32602      | Invalid params                 | Request parameters are missing or malformed                                  |
| 429         | Too Many Requests              | Rate limit exceeded for your plan                                            |
| -32000      | Insufficient fee token balance | Sender (or fee payer) does not have enough of the selected TIP-20 fee token  |
| -32000      | Nonce too low                  | Transaction's nonce has already been used                                    |
| -32000      | Wrong subblock proposer        | Type `0x54` transaction targets a subblock proposer different from this node |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_sendRawTransaction', ["0x02f86c8210798504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."]);
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

const result = await client.request({ method: 'eth_sendRawTransaction', params: ["0x02f86c8210798504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."] });
console.log(result);
```
{% endtab %}
{% endtabs %}
