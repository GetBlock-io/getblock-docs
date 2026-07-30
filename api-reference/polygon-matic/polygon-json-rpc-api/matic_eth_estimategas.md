---
description: >-
  Example code for the eth_estimateGas json-rpc method. Сomplete guide on how to
  use eth_estimateGas json-rpc in GetBlock.io Web3 documentation.
---

# eth\_estimateGas - Polygon

The **eth\_estimateGas** method generates and returns an estimate of how much gas is necessary to allow the transaction to complete. The transaction will not be added to the blockchain. The estimate may be significantly more than the amount of gas actually used by the transaction due to a variety of reasons including EVM mechanics and node performance.

## Parameters

| Parameter      | Type   | Required | Description                                                                 |
| -------------- | ------ | -------- | --------------------------------------------------------------------------- |
| transaction    | object | Yes      | Transaction call object                                                     |
| blockParameter | string | No       | Block number in hex, or "latest", "earliest", "pending" (default: "latest") |

### Transaction Object

| Field    | Type   | Required | Description                                                  |
| -------- | ------ | -------- | ------------------------------------------------------------ |
| from     | string | No       | The address the transaction is sent from                     |
| to       | string | No       | The destination address (required for non-contract creation) |
| gas      | string | No       | Upper limit of gas for the estimation (hex)                  |
| gasPrice | string | No       | Gas price in wei (hex)                                       |
| value    | string | No       | Value to send in wei (hex)                                   |
| data     | string | No       | Contract code or encoded function call                       |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1",
        "to": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0",
        "value": "0x0",
        "data": "0xa9059cbb000000000000000000000000d8dA6BF26964aF9D7eEd9e03E53415D37aA960450000000000000000000000000000000000000000000000056bc75e2d63100000"
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="estimateGas.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_estimateGas',
    params: [{
        from: '0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1',
        to: '0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0',
        value: '0x0',
        data: '0xa9059cbb000000000000000000000000d8dA6BF26964aF9D7eEd9e03E53415D37aA960450000000000000000000000000000000000000000000000056bc75e2d63100000'
    }],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const gasEstimate = parseInt(response.data.result, 16);
    console.log('Estimated Gas:', gasEstimate);
})
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python (requests)" %}
{% code title="estimate_gas.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1",
        "to": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0",
        "value": "0x0",
        "data": "0xa9059cbb000000000000000000000000d8dA6BF26964aF9D7eEd9e03E53415D37aA960450000000000000000000000000000000000000000000000056bc75e2d63100000"
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
result = response.json()
gas_estimate = int(result["result"], 16)
print(f"Estimated Gas: {gas_estimate}")
```
{% endcode %}
{% endtab %}

{% tab title="Rust (reqwest)" %}
{% code title="main.rs" %}
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
            "method": "eth_estimateGas",
            "params": [{
                "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1",
                "to": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0",
                "value": "0x0"
            }],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x5208"
}
```

## Response Parameters

| Field   | Type   | Description                                                              |
| ------- | ------ | ------------------------------------------------------------------------ |
| jsonrpc | string | JSON-RPC version (2.0)                                                   |
| id      | string | Request identifier                                                       |
| result  | string | Estimated gas amount in hexadecimal (0x5208 = 21000 for simple transfer) |

## Use Case

The eth\_estimateGas method is essential for:

* **Transaction preparation**: Determine gas limit before sending transactions
* **Cost estimation**: Calculate transaction costs for users
* **Contract interaction**: Estimate gas for complex smart contract calls
* **Budget planning**: Ensure sufficient funds for transaction execution
* **Error prevention**: Detect transactions that would fail before sending

### Example: Calculate Transaction Cost

```javascript
import { ethers } from 'ethers';

async function estimateTransactionCost(provider, tx) {
    // Get gas estimate
    const gasEstimate = await provider.estimateGas(tx);
    
    // Get current gas price
    const feeData = await provider.getFeeData();
    
    // Calculate total cost
    const maxCost = gasEstimate * feeData.maxFeePerGas;
    const likelyCost = gasEstimate * feeData.gasPrice;
    
    console.log(`Gas Estimate: ${gasEstimate}`);
    console.log(`Max Cost: ${ethers.formatEther(maxCost)} MATIC`);
    console.log(`Likely Cost: ${ethers.formatEther(likelyCost)} MATIC`);
    
    return { gasEstimate, maxCost, likelyCost };
}
```

## Error Handling

| Status Code | Error Message                  | Cause                              |
| ----------- | ------------------------------ | ---------------------------------- |
| 403         | Forbidden                      | Missing or invalid ACCESS-TOKEN    |
| -32000      | execution reverted             | Transaction would fail             |
| -32000      | gas required exceeds allowance | Gas limit too low or infinite loop |
| -32602      | Invalid params                 | Invalid transaction parameters     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const gasEstimate = await provider.estimateGas({
    from: '0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1',
    to: '0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0',
    value: ethers.parseEther('1.0')
});

console.log('Estimated Gas:', gasEstimate.toString());
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http, parseEther } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const gasEstimate = await client.estimateGas({
    account: '0x742d35Cc6634C0532925a3b844Bc9e7595f5bEb1',
    to: '0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0',
    value: parseEther('1')
});

console.log('Estimated Gas:', gasEstimate);
```
{% endcode %}
{% endtab %}
{% endtabs %}
