---
description: >-
  Example code for the eth_gasPrice json-rpc method. Сomplete guide on how to
  use eth_gasPrice json-rpc in GetBlock.io Web3 documentation.
---

# eth\_gasPrice - Polygon

The eth\_gasPrice method returns the current gas price in wei. This value represents the median gas price of the most recent blocks and is useful for estimating transaction costs. On Polygon, gas prices are typically very low compared to Ethereum mainnet.

### Parameters

* None

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_gasPrice",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_gasPrice',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const gasPriceWei = parseInt(response.data.result, 16);
    const gasPriceGwei = gasPriceWei / 1e9;
    console.log(`Gas Price: ${gasPriceGwei} Gwei`);
})
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python (requests)" %}
{% code title="request.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_gasPrice",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
result = response.json()
gas_price_wei = int(result["result"], 16)
gas_price_gwei = gas_price_wei / 1e9
print(f"Gas Price: {gas_price_gwei} Gwei")
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
            "method": "eth_gasPrice",
            "params": [],
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

### Response

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x6FC23AC00"
}
```
{% endcode %}

### Response parameters

| Field   | Type   | Description                            |
| ------- | ------ | -------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                 |
| id      | string | Request identifier                     |
| result  | string | Current gas price in wei (hexadecimal) |

### Use case

The eth\_gasPrice method is essential for:

* Transaction cost calculation: Estimate total cost before sending transactions
* Gas price monitoring: Track network congestion levels
* Dynamic pricing: Adjust gas prices based on network conditions
* Budget management: Ensure sufficient funds for transactions
* Arbitrage: Monitor gas prices for MEV opportunities

#### Example: Transaction Cost Calculator

{% code title="calculateTransactionCost.js" %}
```javascript
async function calculateTransactionCost(provider, gasLimit) {
    const gasPrice = await provider.send('eth_gasPrice', []);
    const gasPriceWei = BigInt(gasPrice);
    const totalCost = gasPriceWei * BigInt(gasLimit);
    
    return {
        gasPrice: gasPriceWei,
        gasPriceGwei: Number(gasPriceWei) / 1e9,
        totalCostWei: totalCost,
        totalCostMatic: Number(totalCost) / 1e18
    };
}
```
{% endcode %}

### Error handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |

### Web3 integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Get fee data (includes gas price)
const feeData = await provider.getFeeData();
console.log('Gas Price:', ethers.formatUnits(feeData.gasPrice, 'gwei'), 'Gwei');

// Using raw RPC call
const gasPrice = await provider.send('eth_gasPrice', []);
console.log('Gas Price (hex):', gasPrice);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http, formatGwei } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const gasPrice = await client.getGasPrice();
console.log('Gas Price:', formatGwei(gasPrice), 'Gwei');
```
{% endcode %}
{% endtab %}
{% endtabs %}
