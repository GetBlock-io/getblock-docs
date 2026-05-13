---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex json-rpc
  in GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Optimism

This method retrieves a specific transaction from a block by combining the block number with the transaction's index position. This is commonly used to iterate through transactions in a block or to access transactions at specific positions. You can use block tags like `'latest'` as well.

## Parameters

| Parameter        | Type          | Description                                                    | Required |
| ---------------- | ------------- | -------------------------------------------------------------- | -------- |
| blockNumber      | QUANTITY\|TAG | Block number in hex, or `'latest'`, `'earliest'`, `'pending'`. | Yes      |
| transactionIndex | QUANTITY      | The transaction index position in hexadecimal.                 | Yes      |

## Request Sample

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": ["0x7A69B2C", "0x0"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Javascript(Axios)" %}
{% code overflow="wrap" %}
```bash
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    method: "eth_getTransactionByBlockNumberAndIndex",
    params: ["0x7A69B2C", "0x0"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getTransactionByBlockNumberAndIndex result:", response.data.result);
        } else {
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        console.error("Error:", error.response ? error.response.data : error.message);
    });
```
{% endcode %}
{% endtab %}

{% tab title="Python(Request)" %}
{% code overflow="wrap" %}
```py
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": ["0x7A69B2C", "0x0"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getTransactionByBlockNumberAndIndex result:", result)
else:
    print("Error:", response.status_code, response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rs
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::builder()
        .build()?;

    let mut headers = reqwest::header::HeaderMap::new();
    headers.insert("Content-Type", "application/json".parse()?);

    let data = r#"{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": ["0x7A69B2C", "0x0"],
    "id": "getblock.io"
}"#;

    let json: serde_json::Value = serde_json::from_str(&data)?;

    let request = client.request(reqwest::Method::POST, "https://go.getblock.us/<ACCESS_TOKEN>")
        .headers(headers)
        .json(&json);

    let response = request.send().await?;
    let body = response.text().await?;

    println!("{}", body);

    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

A successful response returns the following:

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "blockHash": "0x1ff2fcd80ed93d75855364f7e879795dbd5790210c5224ad2c1083d9744297d6",
        "blockNumber": "0x7a69b2c",
        "blockTimestamp": "0x67411011",
        "from": "0xdeaddeaddeaddeaddeaddeaddeaddeaddead0001",
        "gas": "0xf4240",
        "gasPrice": "0x0",
        "hash": "0x4fae8f6c70a4f50cf8894bf9847ed364922826658c3cb73e291e03934637a853",
        "input": "0x440a5e200000146b000f79c500000000000000040000000067410f83000000000144320300000000000000000000000000000000000000000000000000000002fe2cca3c00000000000000000000000000000000000000000000000000000000001f11bf37aac56bf514562b0ee38600119c0a4f70b20f0c6abc66a2011d1bdd9b18cb1c0000000000000000000000006887246668a3b87f54deb3b94ba47a6f63f32985",
        "nonce": "0x160d8b6",
        "to": "0x4200000000000000000000000000000000000015",
        "transactionIndex": "0x0",
        "value": "0x0",
        "type": "0x7e",
        "v": "0x0",
        "r": "0x0",
        "s": "0x0",
        "sourceHash": "0xf14ecb423df1f95f609d9781126cdd9ea4d248f4502c083507b2f1f72aa779a8",
        "mint": "0x0",
        "depositReceiptVersion": "0x1"
    }
}

```
{% endcode %}

## Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: A transaction object with all transaction details, or null if not found.

## Use Case

The eth\_getTransactionByBlockNumberAndIndex method is commonly used for:

* **Transaction iteration**
* **Block processing**
* **Sequential access patterns**

#### Error handling

| Status Code | Error Message    | Cause                                                                                                                                                                      |
| ----------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 404         | Not Found        | Missing or invalid ACCESS\_TOKEN.                                                                                                                                          |
| -32602      | Invalid argument | <ul><li>The block number does not exist or is beyond the chain height</li><li>Index is not a valid hex integer</li><li>Block does have transaction at that index</li></ul> |

#### Integration with Web3

The `eth_getTransactionByBlockNumberAndIndex` method helps developers:

* Build trustless UIs that load transaction lists directly from RPC
* Power dashboards analyzing block activity and congestion
* Support exchanges, wallets, and bridges that track transaction placement
* Generate block-level historical analytics
* Verify transaction ordering for MEV, priority fees, and gas bidding strategies

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send( "eth_getTransactionByBlockNumberAndIndex",
 ["latest", "0x0"]);    
console.log("The result:", result);
    return result;
  } catch (error) {
    console.error("The error:", error);
    throw error;
  }
}
```
{% endtab %}

{% tab title="Viem" %}
```jsx
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: optimism,
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'),
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
         method: "eth_getTransactionByBlockNumberAndIndex",
         params: ["latest", "0x0"]
        });
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }
}

```
{% endtab %}
{% endtabs %}
