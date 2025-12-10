---
description: >-
  Example code for the eth_blockNumber json-rpc method. Сomplete guide on how to
  use eth_blockNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_blockNumber - Arbitrum

This method gets the number of the most recent block.

#### Parameters

* None

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
  "jsonrpc": "2.0",
  "method": "eth_blockNumber",
  "params": [],
  "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
  jsonrpc: "2.0",
  method: "eth_blockNumber",
  params: [],
  id: "getblock.io",
});

let config = {
  method: "post",
  maxBodyLength: Infinity,
  url: "https://go.getblock.us/<ACCESS_TOKEN>",
  headers: {
    "Content-Type": "application/json",
  },
  data: data,
};

axios
  .request(config)
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

url = "https://go.getblock.us/<ACESS_TOKEN>"

payload = json.dumps({
  "jsonrpc": "2.0",
  "method": "eth_blockNumber",
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
{% endtab %}
{% endtabs %}

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x183037af"
}
```

#### Reponse Parameter Definition

| Field    | Data Type | Definition                                                                 |
| -------- | --------- | -------------------------------------------------------------------------- |
| `result` | string    | The block number of the most recently mined block as a hexadecimal string. |

#### Use case

`eth_blockNumber` is used to:

* Track the latest block height
* Monitor chain progress or finality
* Trigger event-based updates when the block number increases

#### Error handling

| Status Code | Error Message | Cause                             |
| ----------- | ------------- | --------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS\_TOKEN. |

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
```javascript
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
{% endtab %}
{% endtabs %}
