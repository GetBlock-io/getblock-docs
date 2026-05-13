---
description: >-
  Example code for the eth_syncing json-rpc method. Сomplete guide on how to use
  eth_syncing json-rpc in GetBlock.io Web3 documentation.
---

# eth\_syncing - Optimism

This method returns information about whether the node is currently syncing with the network. If syncing, it returns an object with details such as the starting block, current block, and highest block. If the node is fully synced, it returns `false`. This is important for ensuring your application is working with current blockchain data.

### Parameters

* None

### Request. Sample

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "eth_syncing",
    "params": [],
    "id": "getblock.io"
}
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "eth_syncing",
  "params": [],
  "id": "getblock.io"
});

let config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.us/<ACCESS_TOKEN>',
  headers: { 
    'Content-Type': 'application/json'
  },
  data : data
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});

```
{% endtab %}

{% tab title="Request" %}
```python
import requests
import json

url = "https://go.getblock.us/<ACCESS_TOKEN>"

payload = json.dumps({
  "jsonrpc": "2.0",
  "method": "eth_syncing",
  "params": [],
  "id": "getblock.io"
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
{% endtab %}

{% tab title="Rust" %}
```rust
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::builder()
        .build()?;

    let mut headers = reqwest::header::HeaderMap::new();
    headers.insert("Content-Type", "application/json".parse()?);

    let data = r#"{
    "jsonrpc": "2.0",
    "method": "eth_syncing",
    "params": [],
    "id": "getblock.io"
}"#;

    let json: serde_json::Value = serde_json::from_str(&data)?;

    let request = client.request(reqwest::Method::POST, "https://go.getblock.us/<ACCESS_TOKEN>")
        .headers(headers)
        .json(&json);

    let response = request.send().await?;
    let body = response.text().await?;

    println!("{}", body);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": false
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: Returns `false` if not syncing, or an object with startingBlock, currentBlock, and highestBlock if syncing.

### Use Case

The eth\_syncing method is commonly used for:

* **Node health monitoring**
* **Sync status checking**
* **Application readiness verification**

#### Error handling

| Status Code | Error Message | Cause                             |
| ----------- | ------------- | --------------------------------- |
| 404         | Not Found     | Missing or invalid ACCESS\_TOKEN. |

#### Integration with Web3

The `eth_syncing` helps developers to:

* Detect if a provider or RPC endpoint is still catching up with the chain.
* Pause transaction execution or contract event listeners until the node is synced.
* Build intelligent dashboards that show syncing progress in real time.
* Improve user experience by avoiding stale reads or outdated contract state.
* Implement logic in wallets, bots and dApps to switch RPC endpoints when one is behind.

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_syncing",
 [
  ]);    
console.log("The result:", result);
    return result;
  } catch (error) {
    console.error("The error:", error);
    throw error;
  }
}
```
{% endtab %}

{% tab title="Viem" %}
```jsx
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: optimism,
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
          method: "eth_syncing",
  params: [
  ],
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
