---
description: >-
  Example code for the eth_estimateGas JSON-RPC method. Complete guide on how to
  use eth_estimateGas JSON-RPC in GetBlock Web3 documentation.
---

# eth\_estimateGas - Mantle

This method generates and returns an estimate of how much gas is necessary to allow a transaction to complete on the Mantle network. On Mantle v2, this includes both L1 and L2 gas costs.

### Parameters

| Parameter      | Type   | Description                                                |
| -------------- | ------ | ---------------------------------------------------------- |
| transaction    | object | Transaction call object                                    |
| blockParameter | string | (optional) Block number or "latest", "earliest", "pending" |

Transaction Object:

| Field    | Type   | Description                 |
| -------- | ------ | --------------------------- |
| from     | string | (optional) Sender address   |
| to       | string | Recipient address           |
| gas      | string | (optional) Gas limit        |
| gasPrice | string | (optional) Gas price in wei |
| value    | string | (optional) Value to send    |
| data     | string | (optional) Transaction data |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "to": "0xD85498dbEaEB1Df24BE52eED4F52eAc2Fbd56245",
        "value": "0xde0b6b3a7640000"
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="estimateGas.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "to": "0xD85498dbEaEB1Df24BE52eED4F52eAc2Fbd56245",
        "value": "0xde0b6b3a7640000"
    }],
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
    "params": [{
        "to": "0xD85498dbEaEB1Df24BE52eED4F52eAc2Fbd56245",
        "value": "0xde0b6b3a7640000"
    }],
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
            "params": [{
                "to": "0xD85498dbEaEB1Df24BE52eED4F52eAc2Fbd56245",
                "value": "0xde0b6b3a7640000"
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

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x5208"
}
```

Response Parameters:

| Field   | Type   | Description                             |
| ------- | ------ | --------------------------------------- |
| jsonrpc | string | JSON-RPC protocol version ("2.0")       |
| id      | string | Request identifier matching the request |
| result  | string | Estimated gas amount in hexadecimal     |

### Use Case

The `eth_estimateGas` method is essential for:

* Transaction gas limit calculation
* Transaction cost estimation
* Smart contract deployment planning
* DeFi transaction preparation
* Wallet gas suggestions
* Preventing out-of-gas errors

### Error Handling

| Status Code | Error Message      | Cause                           |
| ----------- | ------------------ | ------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN |
| -32000      | Execution reverted | Transaction would fail          |
| -32602      | Invalid params     | Invalid transaction object      |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const gasEstimate = await provider.estimateGas({
    to: '0xD85498dbEaEB1Df24BE52eED4F52eAc2Fbd56245',
    value: ethers.parseEther('1.0')
});
console.log('Gas estimate:', gasEstimate.toString());
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, parseEther } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const gasEstimate = await client.estimateGas({
    to: '0xD85498dbEaEB1Df24BE52eED4F52eAc2Fbd56245',
    value: parseEther('1.0')
});
console.log('Gas estimate:', gasEstimate);
```
{% endtab %}
{% endtabs %}
