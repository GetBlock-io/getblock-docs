# eth\_getBalance - Monad

This method returns the balance of an account in MON-wei (the smallest unit of MON).

{% hint style="info" %}
* Balance is returned in MON-wei (1 MON = 10^18 MON-wei)
* Due to asynchronous execution, balance queries at "pending" may not reflect very recent transactions
* For EIP-7702 delegated EOAs, balance cannot be lowered below 10 MON due to Reserve Balance rules
{% endhint %}

### Parameters

| Parameter | Type   | Required | Description                                                                                       |
| --------- | ------ | -------- | ------------------------------------------------------------------------------------------------- |
| address   | string | Yes      | The address to check the balance of (20 bytes).                                                   |
| block     | string | No       | Block number in hex, or "latest", "earliest", "pending", "safe", "finalized" (default: "latest"). |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
     "method": "eth_getBalance",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb", "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
     "method": "eth_getBalance",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb", "latest"],
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
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
     "method": "eth_getBalance",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb", "latest"],
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
{% code overflow="wrap" %}
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
            "method": "eth_getBalance",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb", "latest"],
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
    "result": "0x0234c8a3397aab58"
}
```

### Response Parameters

| Field  | Type   | Description                                     |
| ------ | ------ | ----------------------------------------------- |
| result | string | The balance in MON-wei as a hexadecimal string. |

### Use Case

The `eth_getBalance` method is essential for:

* Wallet applications displaying MON balances
* Checking account funds before transactions
* Portfolio tracking applications
* DeFi protocol balance verification
* Exchange integration for deposits/withdrawals
* Gas payment verification

### Error Handling

| Status Code | Error Message      | Cause                                      |
| ----------- | ------------------ | ------------------------------------------ |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN.           |
| -32602      | Invalid params     | Invalid address format or block parameter. |
| -32000      | Resource not found | Block not found.                           |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function fetchBlockNumberFromProvider() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number)
     const result = await provider.send("eth_getBalance", ["0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb", "latest"],);
    console.log("Block number result:", result);
    return result;
  } catch (error) {
    console.error("Ethers Error fetching block number:", error);
    throw error;
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```js
import { createPublicClient, http } from 'viem';
import { monad } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: monad,
  transport: http("https://go.getblock.us/<ACCESS_TOKEN>"),
});

// Using the method through Viem
async function fetchAccountViem() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
            method: 'eth_blockNumber',
            param: ["0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb", "latest"],
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
