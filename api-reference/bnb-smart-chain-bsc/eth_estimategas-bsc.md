---
description: >-
  Example code for the eth_estimateGas JSON RPC method. Сomplete guide on how to
  use eth_estimateGas JSON RPC in GetBlock Web3 documentation.
---

# eth\_estimateGas - BSC

This method generates an estimate of how much gas is necessary to complete a transaction on the BNB Smart Chain. This is essential for setting appropriate gas limits and calculating transaction costs. BSC's low gas prices make accurate estimation important for cost optimization.

## Parameters

| Parameter   | Type   | Required | Description                                |
| ----------- | ------ | -------- | ------------------------------------------ |
| transaction | object | Yes      | Transaction call object                    |
| blockNumber | string | No       | Block number in hex, or "latest" (default) |

### Transaction Object

| Field    | Type   | Required | Description          |
| -------- | ------ | -------- | -------------------- |
| from     | string | No       | Sender address       |
| to       | string | No       | Recipient address    |
| gas      | string | No       | Gas limit            |
| gasPrice | string | No       | Gas price            |
| value    | string | No       | Value to send in wei |
| data     | string | No       | Contract call data   |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x8D97689C9818892B700e27F316cc3E41e17fBeb9",
        "to": "0x55d398326f99059fF775485246999027B3197955",
        "value": "0x0",
        "data": "0xa9059cbb0000000000000000000000008626f6940e2eb28930efb4cef49b2d1f2c9c11990000000000000000000000000000000000000000000000000de0b6b3a7640000"
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_estimateGas',
    params: [{
        from: '0x8D97689C9818892B700e27F316cc3E41e17fBeb9',
        to: '0x55d398326f99059fF775485246999027B3197955',
        value: '0x0',
        data: '0xa9059cbb0000000000000000000000008626f6940e2eb28930efb4cef49b2d1f2c9c11990000000000000000000000000000000000000000000000000de0b6b3a7640000'
    }],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const gasEstimate = parseInt(response.data.result, 16);
    console.log('Gas Estimate:', gasEstimate);
})
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x8D97689C9818892B700e27F316cc3E41e17fBeb9",
        "to": "0x55d398326f99059fF775485246999027B3197955",
        "value": "0x0",
        "data": "0xa9059cbb0000000000000000000000008626f6940e2eb28930efb4cef49b2d1f2c9c11990000000000000000000000000000000000000000000000000de0b6b3a7640000"
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
gas_estimate = int(response.json()["result"], 16)
print(f"Gas Estimate: {gas_estimate}")
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="request.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_estimateGas",
        "params": [{
            "from": "0x8D97689C9818892B700e27F316cc3E41e17fBeb9",
            "to": "0x55d398326f99059fF775485246999027B3197955",
            "value": "0x0"
        }],
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0xc350"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                            |
| --------- | ------ | -------------------------------------- |
| jsonrpc   | string | JSON-RPC version (2.0)                 |
| id        | string | Request identifier                     |
| result    | string | Estimated gas in hex (0xc350 = 50,000) |

## Use Cases

* Calculate transaction costs before sending
* Set appropriate gas limits for BEP-20 transfers
* Budget gas for PancakeSwap swaps
* Optimize DeFi transaction parameters
* Prevent out-of-gas errors

## Error Handling

| Error Code | Description                                    |
| ---------- | ---------------------------------------------- |
| -32602     | Invalid params - malformed transaction object  |
| -32603     | Internal error - execution reverted            |
| -32000     | Execution error - insufficient funds or revert |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const gasEstimate = await provider.estimateGas({
    from: '0x8D97689C9818892B700e27F316cc3E41e17fBeb9',
    to: '0x55d398326f99059fF775485246999027B3197955',
    value: ethers.parseEther('0.1')
});
console.log('Gas Estimate:', gasEstimate.toString());
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http, parseEther } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const gasEstimate = await client.estimateGas({
    account: '0x8D97689C9818892B700e27F316cc3E41e17fBeb9',
    to: '0x55d398326f99059fF775485246999027B3197955',
    value: parseEther('0.1')
});
console.log('Gas Estimate:', gasEstimate);
```
{% endcode %}
{% endtab %}
{% endtabs %}
