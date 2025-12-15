---
description: >-
  Example code for the eth_newFilter JSON RPC method. Сomplete guide on how to
  use eth_newFilter JSON RPC in GetBlock Web3 documentation.
---

# eth\_newFilter - Arbitrum

This method creates a filter that notifies the client when matching logs are generated

#### Parameters

* Object

| Field     | Type            | Description                                                         |
| --------- | --------------- | ------------------------------------------------------------------- |
| fromBlock | string          | Block height to start searching from. Accepts hex or tags.          |
| toBlock   | string          | Block height to stop searching at. Accepts hex or tags.             |
| address   | string or array | Address or list of addresses to filter log events from.             |
| topics    | array           | Topics used to match specific log signatures or indexed parameters. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "eth_newFilter",
    "params": [
        {
            "fromBlock": "earliest",
            "toBlock": "latest",
            "topics": []
        }]
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
    "jsonrpc": "2.0",
       "method": "eth_newFilter",
    "params": [
        {
            "fromBlock": "earliest",
            "toBlock": "latest",
            "topics": []
        }]
    "id": "getblock.io"
};

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
    "method": "eth_newFilter",
    "params": [
        {
            "fromBlock": "earliest",
            "toBlock": "latest",
            "topics": []
        }]
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
      "method": "eth_newFilter",
    "params": [
        {
            "fromBlock": "earliest",
            "toBlock": "latest",
            "topics": []
        }]
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
    "result": "0xc44f6d615fb0692b014b68ae15428371"
}
```

#### Reponse Parameter Definition

| Field  | Description                                                     | Data Type |
| ------ | --------------------------------------------------------------- | --------- |
| result | A hexadecimal string representing the ID of the created filter. | String    |

#### Use case

The `eth_newFilter` method helps developers to:

* Subscribe to smart contract events
* Build dashboards or indexers that react to chain activity
* Monitor ERC20 transfer logs
* Track DeFi protocol events like swaps or deposits
* Support NFT mint/burn/listing activity
* Build real-time analytics with low RPC load

#### Error handling

| Status Code | Error Message    | Cause                                                          |
| ----------- | ---------------- | -------------------------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.                              |
| -32602      | Invalid argument | <ul><li>Invalid filter parameters</li><li>Rate limit</li></ul> |

#### Integration with Web3

Using `eth_newFilter` allows developers to:

* Build interfaces that listen for contract events without needing WebSocket support
* Implement background polling workers for analytics dashboards
* Detect state changes in dapps and update UI instantly
* Create automated bots such as liquidation bots or claim bots
* Watch ERC20, ERC721, and custom events in a trustless way

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send( "eth_newFilter",
[
  {
    fromBlock: "earliest",
    toBlock: "latest",
    topics: [],
  },
],);    
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
          method: "eth_newFilter",
          params: [
            {
              fromBlock: "earliest",
              toBlock: "latest",
              topics: [],
            },
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
