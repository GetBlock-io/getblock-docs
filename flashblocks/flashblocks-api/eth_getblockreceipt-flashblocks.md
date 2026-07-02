---
description: >-
  Example code for the eth_getBlockReceipt  Flashblocks method. Complete guide
  on how to use eth_getBlockReceipt Flashblocks in GetBlock Web3 documentation.
---

# eth\_getBlockReceipt - Flashblocks

This returns the receipt of a block — preconfirmed OR finalized. Preconfirmed receipts are identifiable by their zero `blockHash` (`0x00...00`) and `blockNumber: null`. Once the parent block seals (within 2 seconds), the same receipt returns with a real block hash. This zero-hash convention is the canonical way to distinguish preconfirmed vs finalized receipts without additional RPC calls.

## Parameters

| Parameter   | Type     | Required | Description |
| ----------- | -------- | -------- | ----------- |
| blockNumber | QUANTITY | TAG      | Yes         |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getBlockReceipts",
  "params": [
    "pending"
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
  "method": "eth_getBlockReceipts",
  "params": [
    "pending"
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

payload = json.dumps({{
  "jsonrpc": "2.0",
  "method": "eth_getBlockReceipts",
  "params": [
    "pending"
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
  "method": "eth_getBlockReceipts",
  "params": [
    "pending"
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
    "result": [
        {
            "type": "0x7e",
            "status": "0x1",
            "cumulativeGasUsed": "0xb496",
            "logs": [],
            "depositNonce": "0x2ddfd39",
            "depositReceiptVersion": "0x1",
            "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "transactionHash": "0xd6586cd3a1cf6f2a79dfdc513863cbaff925578131dc9e42872acdc3dab4aee9",
            "transactionIndex": "0x0",
            "blockHash": "0xe9526259318b13325e3926a3d29a4323d53d7e3c25ebfcf6b0f7ace7376af639",
            "blockNumber": "0x2ddfd36",
            "gasUsed": "0xb496",
            "effectiveGasPrice": "0x0",
            "blobGasUsed": "0x41e8",
            "from": "0xdeaddeaddeaddeaddeaddeaddeaddeaddead0001",
            "to": "0x4200000000000000000000000000000000000015",
            "contractAddress": null,
            "l1GasPrice": "0x1564212e",
            "l1GasUsed": "0x72a",
            "l1Fee": "0x0",
            "l1BaseFeeScalar": "0x8dd",
            "l1BlobBaseFee": "0x1571d4a",
            "l1BlobBaseFeeScalar": "0x101c12",
            "daFootprintGasScalar": "0x94"
  }
   ],
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field                    | Type         | Description                                                                      |
| ------------------------ | ------------ | -------------------------------------------------------------------------------- |
| result.blockHash         | DATA         | `0x00...00` (zero hash) for preconfirmed receipts; real block hash for finalized |
| result.blockNumber       | QUANTITY     | `null` for preconfirmed; block number in hex for finalized                       |
| result.status            | QUANTITY     | `0x1` = success, `0x0` = reverted                                                |
| result.gasUsed           | QUANTITY     | Actual gas consumed                                                              |
| result.effectiveGasPrice | QUANTITY     | Effective gas price paid                                                         |
| result.logs              | array of Log | Event logs emitted during execution                                              |

## Use Cases

* Wallet UIs showing preconfirmed transaction success within 200ms of submission
* Polling for confirmed inclusion at 200ms cadence instead of 2s
* Distinguishing 'transaction preconfirmed' vs 'transaction finalized' UX states
* Detecting transaction revert or success before block seal

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call — 'pending' returns Flashblocks-preconfirmed state:
const result = await provider.send('eth_getBlockReceipts', ["pending"]);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider that accept 'pending':
// provider.getBalance(addr, 'pending'), provider.getTransactionCount(addr, 'pending'), etc.
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// viem's read methods accept blockTag: 'pending' for Flashblocks-preconfirmed state:
const result = await client.request({
    method: 'eth_getBlockReceipts',
    params: ["pending"]
});
console.log(result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
