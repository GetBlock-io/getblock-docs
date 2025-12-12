---
description: >-
  Example code for the eth_syncing json-rpc method. Сomplete guide on how to use
  eth_syncing json-rpc in GetBlock.io Web3 documentation.
---

# eth\_syncing - Arbitrum

This method returns the node's syncing status. If the node is fully synced, it returns `false`. If syncing is in progress, it returns an object describing the current sync state such as starting block, current block, highest block, and related fields. This method helps developers understand whether the node is caught up with the blockchain.

#### Parameters

* none

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "eth_signTransaction",
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

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": false
}
```

#### Response Parameter Definition

| Field               | Type              | Description                                                               |
| ------------------- | ----------------- | ------------------------------------------------------------------------- |
| result              | boolean or object | `false` if node is fully synced. Otherwise, an object with sync progress. |
| startingBlock       | string            | Block number where syncing started.                                       |
| currentBlock        | string            | Block that the node is currently processing.                              |
| highestBlock        | string            | The latest block known to the network.                                    |
| warpChunksAmount    | string            | Total warp sync chunks required for full sync.                            |
| warpChunksProcessed | string            | Number of warp chunks processed so far.                                   |
| pulledStates        | string            | Number of state entries downloaded.                                       |
| knownStates         | string            | Total expected state entries.                                             |

#### Error handling

| Status Code | Error Message | Cause                             |
| ----------- | ------------- | --------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS\_TOKEN. |

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
import { arbitrum } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: arbitrum,
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
