---
description: >-
  Example code for the eth_createAccessList JSON-RPC method. Complete guide on
  how to use eth_createAccessList JSON-RPC in GetBlock Web3 documentation.
---

# eth\_createAccessList - Monad

This method creates an EIP-2930 access list that describes all storage slots and addresses accessed during a transaction simulation.

## Parameters

| Parameter            | Type   | Required | Description                                              |
| -------------------- | ------ | -------- | -------------------------------------------------------- |
| transaction          | object | Yes      | The transaction call object.                             |
| transaction.from     | string | No       | The address the transaction is sent from.                |
| transaction.to       | string | Yes      | The address the transaction is directed to.              |
| transaction.gas      | string | No       | Gas provided for the call (hex).                         |
| transaction.gasPrice | string | No       | Gas price in wei (hex).                                  |
| transaction.value    | string | No       | Value sent with the call (hex).                          |
| transaction.data     | string | No       | Hash of the method signature and encoded parameters.     |
| blockNumber          | string | No       | Block number in hex, or "latest", "earliest", "pending". |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_createAccessList",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb",
        "to": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
        "data": "0xa9059cbb000000000000000000000000..."
    }, "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="eth_createAccessList.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_createAccessList",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb",
        "to": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
        "data": "0xa9059cbb000000000000000000000000..."
    }, "latest"],
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
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="eth_createAccessList.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_createAccessList",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb",
        "to": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
        "data": "0xa9059cbb000000000000000000000000..."
    }, "latest"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="eth_createAccessList.rs" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "eth_createAccessList",
            "params": [{
                "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb",
                "to": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
                "data": "0xa9059cbb000000000000000000000000..."
            }, "latest"],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "accessList": [
            {
                "address": "0xdac17f958d2ee523a2206206994597c13d831ec7",
                "storageKeys": [
                    "0x0000000000000000000000000000000000000000000000000000000000000000",
                    "0x0000000000000000000000000000000000000000000000000000000000000001"
                ]
            }
        ],
        "gasUsed": "0x5208"
    }
}
```
{% endcode %}

## Response Parameters

| Field                     | Type   | Description                                       |
| ------------------------- | ------ | ------------------------------------------------- |
| accessList                | array  | Array of addresses and storage keys accessed.     |
| accessList\[].address     | string | Contract address accessed.                        |
| accessList\[].storageKeys | array  | Array of storage slots accessed in that contract. |
| gasUsed                   | string | Estimated gas with the access list.               |

## Use Case

The `eth_createAccessList` method is essential for:

* Gas optimization with EIP-2930 transactions
* Pre-warming storage slots
* Transaction simulation analysis
* Gas cost reduction strategies
* DeFi transaction optimization
* Understanding contract interactions

## Error Handling

| Status Code | Error Message      | Cause                            |
| ----------- | ------------------ | -------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN. |
| -32602      | Invalid params     | Invalid transaction parameters.  |
| -32000      | Execution reverted | Transaction would revert.        |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-createAccessList.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const tx = {
    from: senderAddress,
    to: contractAddress,
    data: encodedFunctionData
};

const result = await provider.send('eth_createAccessList', [tx, 'latest']);

console.log('Access list:', result.accessList);
console.log('Gas with access list:', parseInt(result.gasUsed, 16));

// Use in transaction
const txWithAccessList = {
    ...tx,
    type: 1, // EIP-2930 transaction
    accessList: result.accessList
};
```
{% endcode %}
{% endtab %}
{% endtabs %}
