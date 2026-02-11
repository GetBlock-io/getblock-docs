---
description: >-
  Example code for the eth_estimateGas JSON-RPC method. Complete guide on how to
  use eth_estimateGas JSON-RPC in GetBlock Web3 documentation.
---

# eth\_estimategas - Worldchain

Generates and returns an estimate of the gas required to complete the transaction on the World Chain network.

### Parameters

| Parameter   | Type   | Description                                                                    |
| ----------- | ------ | ------------------------------------------------------------------------------ |
| transaction | object | Transaction call object with to, data, and optional from, gas, gasPrice, value |
| blockNumber | string | (Optional) Block number in hex, or "latest", "earliest", "pending"             |

### Request

{% tabs %}
{% tab title="curl" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc454e4438f44e", "to": "0x1234567890123456789012345678901234567890", "value": "0x9184e72a000"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="estimateGas.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc454e4438f44e", "to": "0x1234567890123456789012345678901234567890", "value": "0x9184e72a000"}],
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

{% tab title="Python (requests)" %}
{% code title="estimate_gas.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc454e4438f44e", "to": "0x1234567890123456789012345678901234567890", "value": "0x9184e72a000"}],
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
            "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc454e4438f44e", "to": "0x1234567890123456789012345678901234567890", "value": "0x9184e72a000"}],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x5208"
}
```

### Response Parameters

| Field  | Type   | Description                                            |
| ------ | ------ | ------------------------------------------------------ |
| result | string | Hexadecimal representation of the estimated gas amount |

### Use Case

The `eth_estimateGas` method on World Chain is typically used for:

* Gas estimation
* Transaction cost calculation
* Gas limit setting
* Fee optimization

### Error Handling

| Status Code | Error Message | Cause                           |
| ----------- | ------------- | ------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-estimate.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const gasEstimate = await provider.estimateGas({
    from: '0x742d35Cc6634C0532925a3b844Bc454e4438f44e',
    to: '0x1234567890123456789012345678901234567890',
    value: ethers.parseEther('0.001')
});
console.log('Gas Estimate:', gasEstimate.toString());
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-estimate.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { worldchain } from 'viem/chains';

const client = createPublicClient({
    chain: worldchain,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const gasEstimate = await client.estimateGas({
    account: '0x742d35Cc6634C0532925a3b844Cc454e4438f44e',
    to: '0x1234567890123456789012345678901234567890',
    value: parseEther('0.001')
});
console.log('Gas Estimate:', gasEstimate);
```
{% endcode %}
{% endtab %}
{% endtabs %}
