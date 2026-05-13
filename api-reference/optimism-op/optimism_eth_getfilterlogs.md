---
description: >-
  Example code for the eth_getFilterLogs json-rpc method. Сomplete guide on how
  to use eth_getFilterLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getFilterLogs - Optimism

This method retrieves all logs that match the filter's criteria, not just those changed since the last poll. This is useful when you need to retrieve all historical logs that match a filter's criteria. Note that this only works with filters created by eth\_newFilter.

### Parameters

| Parameter | Type     | Description                               | Required |
| --------- | -------- | ----------------------------------------- | -------- |
| filterId  | QUANTITY | The filter ID returned by eth\_newFilter. | Yes      |

### Request Sample

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterLogs",
    "params": ["0x6c8b2705baa8223048745077aa6202d0"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Javascript(axios)" %}
```bash
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    method: "eth_getFilterLogs",
    params: ["0x6c8b2705baa8223048745077aa6202d0"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getFilterLogs result:", response.data.result);
        } else {
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        console.error("Error:", error.response ? error.response.data : error.message);
    });
```
{% endtab %}

{% tab title="Python(Request)" %}
{% code overflow="wrap" %}
```py
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_getFilterLogs",
    "params": ["0x6c8b2705baa8223048745077aa6202d0"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getFilterLogs result:", result)
else:
    print("Error:", response.status_code, response.text)
```
{% endcode %}
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
    "method": "eth_getFilterLogs",
    "params": ["0x6c8b2705baa8223048745077aa6202d0"],
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "address": "0x4200000000000000000000000000000000000006",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000478946bcd4a5a22b316470f5486fafb928c0ba25",
                "0x0000000000000000000000009e3ed65340a913b96ba7b86d5d5876dde623946e"
            ],
            "data": "0x00000000000000000000000000000000000000000000000051d1a85fe050a227",
            "blockNumber": "0x9084931",
            "transactionHash": "0x2244db1f6eba603c0c41213d945b30f24d84eb16832d5b25e6d4e65df986f6ea",
            "transactionIndex": "0x3",
            "blockHash": "0x19b211ff269fec58f6e57a0233095c88deef1d6909078de93ae892febe6d24c5",
            "blockTimestamp": "0x6a046c1b",
            "logIndex": "0x11",
            "removed": false
        }
    ]
}

```
{% endcode %}

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: An array of all log objects matching the filter criteria.

### Use Case

The eth\_getFilterLogs method is commonly used for:

* **Historical log retrieval**
* **Complete event history**
* **Filter result aggregation**

### Error handling

| Status Code | Error Message    | Cause                                                                                                                                 |
| ----------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| 404         | Not Found        | Missing or invalid ACCESS\_TOKEN.                                                                                                     |
| -32000      | Invalid argument | <ul><li>The Address not 20 bytes or missing prefix</li><li>block hash isn't accurate or incomplete </li><li>keyword missing</li></ul> |

#### Integration with Web3

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getFilterChanges",
     ["0x6c8b2705baa8223048745077aa6202d0"],);    
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
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'),
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
          "method": "eth_getFilterChanges",
    "params": ["0x6c8b2705baa8223048745077aa6202d0"]
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
