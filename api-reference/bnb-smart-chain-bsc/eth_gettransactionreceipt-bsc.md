---
description: >-
  Example code for the eth_getTransactionReceipt JSON RPC method. Сomplete guide
  on how to use eth_getTransactionReceipt JSON RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionReceipt - BSC

This method returns the receipt of a transaction by transaction hash on the BNB Smart Chain. The receipt contains information about the execution of the transaction including status, gas used, and logs.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | 32-byte transaction hash |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionReceipt",
    "params": ["0xcaf9dc7f41315007d9a3644196aacc2b033efabc7ca4dd6dc3b8f2597247df35"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getTransactionReceipt',
    params: ['0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b'],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const receipt = response.data.result;
    console.log('Status:', receipt.status === '0x1' ? 'Success' : 'Failed');
    console.log('Gas Used:', parseInt(receipt.gasUsed, 16));
})
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getTransactionReceipt",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
receipt = response.json()["result"]
print(f"Status: {'Success' if receipt['status'] == '0x1' else 'Failed'}")
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_getTransactionReceipt",
        "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
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
{% endcode %}
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
        "contractAddress": null,
        "cumulativeGasUsed": "0x8bd8",
        "effectiveGasPrice": "0x0",
        "from": "0x9fb20230831d9051c67cc2527d7bf0ea51e5412e",
        "gasUsed": "0x8bd8",
        "logs": [],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1",
        "to": "0x4848489f0b2bedd788c696e2d79b6b69d7484848",
        "transactionHash": "0xcaf9dc7f41315007d9a3644196aacc2b033efabc7ca4dd6dc3b8f2597247df35",
        "transactionIndex": "0x0",
        "type": "0x0"
    }
}
```
{% endcode %}

## Response Parameters

| Field           | Type   | Description                    |
| --------------- | ------ | ------------------------------ |
| status          | string | 0x1 (success) or 0x0 (failure) |
| transactionHash | string | Transaction hash               |
| blockHash       | string | Block hash                     |
| blockNumber     | string | Block number                   |
| gasUsed         | string | Gas used by this tx            |
| logs            | array  | Array of log objects           |
| status          | string | Execution status               |
| gasUsed         | string | Gas consumed                   |
| logs            | array  | Event logs emitted             |

## Use Cases

* Verify transaction success
* Calculate actual gas costs
* Extract event logs
* Confirm BEP-20 transfers

## Error Handling

<details>

<summary>-32602 — Invalid params</summary>

The request parameters are invalid.

</details>

<details>

<summary>null result — Transaction not found or pending</summary>

A null result indicates the transaction is not found in the chain yet or is still pending.

</details>

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const receipt = await provider.getTransactionReceipt('0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b');
console.log('Status:', receipt.status === 1 ? 'Success' : 'Failed');
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const receipt = await client.getTransactionReceipt({
    hash: '0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b'
});
console.log('Status:', receipt.status);
```
{% endcode %}
{% endtab %}
{% endtabs %}
