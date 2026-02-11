---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex JSON RPC method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex JSON RPC
  in GetBlock Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - BSC

This method returns information about a transaction by block number and transaction index position on the BNB Smart Chain.

## Parameters

| Parameter   | Type   | Required | Description                    |
| ----------- | ------ | -------- | ------------------------------ |
| blockNumber | string | Yes      | Block number (hex) or "latest" |
| index       | string | Yes      | Transaction index (hex)        |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": ["latest", "0x0"],
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
    method: 'eth_getTransactionByBlockNumberAndIndex',
    params: ['latest', '0x0'],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": ["latest", "0x0"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
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
        "method": "eth_getTransactionByBlockNumberAndIndex",
        "params": ["latest", "0x0"],
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
        "blockHash": "0x5cd602c9365e1f343cfeb50d7ab71f8b38efd397fa4b41c42fb54772b5ec5416",
        "blockNumber": "0x4a883f0",
        "from": "0x9fb20230831d9051c67cc2527d7bf0ea51e5412e",
        "gas": "0xbbb0",
        "gasPrice": "0x0",
        "hash": "0xcaf9dc7f41315007d9a3644196aacc2b033efabc7ca4dd6dc3b8f2597247df35",
        "input": "0x5b3438636c75622d7075697373616e742d6275696c6465725d2069643a2032623362343235392d373139612d386162642d646233392d3563303533356562343837320a0a2a2032342d486f7572204275696c6465722050726f706f73616c20526174650a2d204d61696e6e65743a2035382e3725205b20313132363334202f20313931393337205d0a2d205468697356616c3a2034342e3425205b2033373337202f2038343234205d0a0a2a20372d446179204275696c6465722050726f706f73616c20526174650a2d204d61696e6e65743a2035342e3825205b20373335353733202f2031333432383535205d0a2d205468697356616c3a2034302e3125205b203232393230202f203537313833205d0a0a2a205265736f75726365730a2d20446f63733a2068747470733a2f2f646f63732e34382e636c75622f7075697373616e742d6275696c6465720a2d2054656c656772616d3a2068747470733a2f2f742e6d652f696e6672615f3438636c7562",
        "nonce": "0x0",
        "to": "0x4848489f0b2bedd788c696e2d79b6b69d7484848",
        "transactionIndex": "0x0",
        "value": "0x0",
        "type": "0x0",
        "chainId": "0x38",
        "v": "0x93",
        "r": "0x30bdd015c19010f96460dcd675608bc4e48514d665cbe0cc2ea77979106e8bdf",
        "s": "0x6aef883f78b92bdb34ef0881bdf38573f75e56a77b4f10ae69b2fbc47bbf2afb"
    }
}
```
{% endcode %}

## Response Parameters Definition

| Field            | Type           | Description                                          |
| ---------------- | -------------- | ---------------------------------------------------- |
| hash             | string         | Hash of the transaction                              |
| blockHash        | string         | Hash of the block this transaction belongs to        |
| blockNumber      | string (hex)   | Block number                                         |
| from             | string         | Sender address                                       |
| to               | string or null | Receiver address or null for contract creation       |
| value            | string (hex)   | Value transferred in wei                             |
| nonce            | string (hex)   | Number of transactions previously sent by the sender |
| gas              | string (hex)   | Gas limit                                            |
| gasPrice         | string (hex)   | Gas price in wei                                     |
| input            | string (hex)   | Transaction calldata                                 |
| transactionIndex | string (hex)   | Index of the transaction inside the block            |
| v, r, s          | string         | ECDSA signature values                               |

## Use Cases

* Access transactions by position
* Build block explorers
* Analyze block contents

## Error Handling

| Error Code  | Description           |
| ----------- | --------------------- |
| -32602      | Invalid params        |
| null result | Transaction not found |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.getBlock('latest', true);
const tx = block.prefetchedTransactions[0];
console.log(tx);
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

const tx = await client.getTransaction({
    blockTag: 'latest',
    index: 0
});
console.log(tx);
```
{% endtab %}
{% endtabs %}
