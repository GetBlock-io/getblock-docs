---
description: >-
  Example code for the eth_sendRawTransactionSync JSON-RPC method. Complete
  guide on how to use eth_sendRawTransactionSync JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_sendRawTransactionSync - Monad

**This is a Monad-specific method (EIP-7966).** Unlike `eth_sendRawTransaction`, this method waits for the transaction to be included in a block before returning.

## Parameters

| Parameter | Type   | Required | Description                                  |
| --------- | ------ | -------- | -------------------------------------------- |
| data      | string | Yes      | The signed transaction data as a hex string. |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransactionSync",
    "params": ["0xf86c0a8502540be400825208944bbeeb066ed09b7aed07bf39eee0460dfa26152088016345785d8a0000801ba0e2c.."],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const signedTx = "0xf86c0a8502540be400825208944bbeeb066ed09b7aed07bf39eee0460dfa26152088016345785d8a0000801ba0e2c..";

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransactionSync",
    "params": [signedTx],
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
{% endtab %}

{% tab title="Requesr" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

signed_tx = "0xf86c0a8502540be400825208944bbeeb066ed09b7aed07bf39eee0460dfa26152088016345785d8a0000801ba0e2c.."

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransactionSync",
    "params": [signed_tx],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust" %}
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
            "method": "eth_sendRawTransactionSync",
            "params": ["0xf86c0a8502540be400825208944bbeeb066ed09b7aed07bf39eee0460dfa26152088016345785d8a0000801ba0e2c.."],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "transactionHash": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
        "blockHash": "0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae",
        "blockNumber": "0x1b4"
    }
}
```

## Response Parameters

| Field           | Type   | Description                                                 |
| --------------- | ------ | ----------------------------------------------------------- |
| transactionHash | string | The 32-byte transaction hash.                               |
| blockHash       | string | The 32-byte hash of the block containing the transaction.   |
| blockNumber     | string | The block number in hex where the transaction was included. |

## Use Case

The `eth_sendRawTransactionSync` method is essential for:

* Applications requiring immediate confirmation feedback
* High-frequency trading where confirmation timing is critical
* Payment processing systems
* Sequential transaction dependencies
* Atomic operation workflows
* Simplified transaction submission without polling

## Error Handling

| Status Code | Error Message        | Cause                                    |
| ----------- | -------------------- | ---------------------------------------- |
| 403         | Forbidden            | Missing or invalid ACCESS-TOKEN.         |
| -32602      | Invalid params       | Invalid transaction data format.         |
| -32000      | Nonce too low        | Transaction nonce already used.          |
| -32000      | Insufficient funds   | Not enough MON for gas.                  |
| -32000      | Transaction timeout  | Transaction not included within timeout. |
| -32003      | Transaction rejected | Transaction validation failed.           |

## Comparison with eth\_sendRawTransaction

| Feature        | eth\_sendRawTransaction      | eth\_sendRawTransactionSync |
| -------------- | ---------------------------- | --------------------------- |
| Returns        | Immediately after submission | After block inclusion       |
| Response       | Transaction hash only        | Hash + block info           |
| Polling needed | Yes                          | No                          |
| Timeout        | N/A                          | Yes (server-defined)        |
| Best for       | Fire-and-forget              | Confirmation-critical ops   |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Use eth_sendRawTransactionSync for sequential operations
async function sendSyncTransaction(signedTx) {
    const result = await provider.send('eth_sendRawTransactionSync', [signedTx]);
    console.log('Confirmed in block:', parseInt(result.blockNumber, 16));
    return result;
}

// First transaction
const result1 = await sendSyncTransaction(signedTx1);

// Second transaction (can safely use incremented nonce)
const result2 = await sendSyncTransaction(signedTx2);
```
{% endtab %}
{% endtabs %}

