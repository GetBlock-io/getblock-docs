---
description: >-
  Example code for the eth_blockNumber json-rpc method. Сomplete guide on how to
  use eth_blockNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_blockNumber - Optimism

The **eth\_blockNumber** method retrieves the current block number. This method is particularly useful for monitoring transaction confirmations, tracking block production, and ensuring applications stay up-to-date with the current chain height.

### Parameters

* None

### Request Sample

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Javascript(Axios)" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    method: "eth_blockNumber",
    params: [],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_blockNumber result:", response.data.result);
        } else {
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        console.error("Error:", error.response ? error.response.data : error.message);
    });
```
{% endcode %}
{% endtab %}

{% tab title="Python(Request)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_blockNumber result:", result)
else:
    print("Error:", response.status_code, response.text)
```
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rs
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::builder()
        .build()?;

    let mut headers = reqwest::header::HeaderMap::new();
    headers.insert("Content-Type", "application/json".parse()?);

    let data = r#"{
    "jsonrpc": "2.0",
     "method": "eth_blockNumber",
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
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x7A69B2C"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The current block number as a hexadecimal string. `0x7A69B2C` in decimal equals 128,449,324.

### Use Case

The `eth_blockNumber` method is commonly used for:

* **Transaction monitoring**
* **Synchronization checks**
* **Block-based triggers**
* **Confirmation counting**

#### Error handling

| Status Code | Error Message | Cause                             |
| ----------- | ------------- | --------------------------------- |
| 404         | Not found     | Missing or invalid ACCESS\_TOKEN. |

#### Integration with Web3

The `eth_blockNumber` can help developers to:

* Polling the chain every few seconds
* Syncing contract state
* Updating dashboards (TVL, gas metrics, transactions)

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function fetchBlockNumberFromProvider() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number)
     const result = await provider.send("eth_blockNumber", []);
    console.log("Block number result:", result);
    return result;
  } catch (error) {
    console.error("Ethers Error fetching block number:", error);
    throw error;
  }
}
```
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
```js
import { createPublicClient, http } from 'viem';
import { arbitrum } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: arbitrum,
  transport: http("https://go.getblock.us/<ACCESS_TOKEN>"),
});

// Using the method through Viem
async function fetchAccountViem() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
            method: 'eth_blockNumber',
        });
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
