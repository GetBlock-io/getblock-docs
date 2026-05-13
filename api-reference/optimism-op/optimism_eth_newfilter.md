---
description: >-
  Example code for the eth_newFilter json-rpc method. Сomplete guide on how to
  use eth_newFilter json-rpc in GetBlock.io Web3 documentation.
---

# eth\_newFilter - Optimism

This method creates a filter that can be polled for new log events matching the specified criteria. The filter can be configured with address, topic, and block-range parameters. Once created, use eth\_getFilterChanges to retrieve new logs that match the filter. Filters timeout if not polled regularly.

### Parameters

| Parameter    | Type   | Description                                                                 | Required |
| ------------ | ------ | --------------------------------------------------------------------------- | -------- |
| filterObject | Object | Filter object with optional fromBlock, toBlock, address, and topics fields. | Yes      |

### Request sample

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
            "fromBlock": "latest",
            "address": "0x4200000000000000000000000000000000000006",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
    ],
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
            "fromBlock": "latest",
            "address": "0x4200000000000000000000000000000000000006",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
    ],
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
    "param":[
        {
            "fromBlock": "latest",
            "address": "0x4200000000000000000000000000000000000006",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
    ],        }]
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
            "fromBlock": "latest",
            "address": "0x4200000000000000000000000000000000000006",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
    ],
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
    "result": "0xfd9d8fda4aa66371b08ac6366a0fa171"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: A filter ID that can be used with eth\_getFilterChanges and eth\_uninstallFilter.

### Use Case

The eth\_newFilter method is commonly used for:

* **Event monitoring**
* **Log polling**
* **Real-time event tracking**
* **Contract event subscription**

#### Error handling

| Status Code | Error Message    | Cause                                                          |
| ----------- | ---------------- | -------------------------------------------------------------- |
| 404         | Not Found        | Missing or invalid ACCESS\_TOKEN.                              |
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
     const result = await provider.send( "eth_newFilter",[
        {
            "fromBlock": "latest",
            "address": "0x4200000000000000000000000000000000000006",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
          method: "eth_newFilter",
    "params": [
        {
            "fromBlock": "latest",
            "address": "0x4200000000000000000000000000000000000006",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
