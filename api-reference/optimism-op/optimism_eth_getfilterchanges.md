---
description: >-
  Example code for the eth_getFilterChanges json-rpc method. Сomplete guide on
  how to use eth_getFilterChanges json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getFilterChanges - Optimism

This method retrieves changes for a filter created by eth\_newFilter, eth\_newBlockFilter, or eth\_newPendingTransactionFilter. The type of data returned depends on the filter type: logs for log filters, block hashes for block filters, or transaction hashes for pending transaction filters.

### Parameters

| Parameter | Type     | Description                                        | Required |
| --------- | -------- | -------------------------------------------------- | -------- |
| filterId  | QUANTITY | The filter ID returned by filter creation methods. | Yes      |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterChanges",
    "params": ["0x6c8b2705baa8223048745077aa6202d0"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    method: "eth_getFilterChanges",
    params: ["0x6c8b2705baa8223048745077aa6202d0"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getFilterChanges result:", response.data.result);
        } else {
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        console.error("Error:", error.response ? error.response.data : error.message);
    });
```
{% endtab %}

{% tab title="Request" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_getFilterChanges",
    "params": ["0x6c8b2705baa8223048745077aa6202d0"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getFilterChanges result:", result)
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
    "method": "eth_getFilterChanges",
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
            "data": "0x00000000000000000000000000000000000000000000000051d1a85fe050a203",
            "blockNumber": "0x9084924",
            "transactionHash": "0xb2adcd1e6d459f23c716c938c8e909f5b622dad31d213637eeccf9dac7d114bc",
            "transactionIndex": "0x3",
            "blockHash": "0x1dd45b1d28600add983fa7b999246f10898329781d3338841fdf721a3a542510",
            "blockTimestamp": "0x6a046c01",
            "logIndex": "0x22",
            "removed": false
        },
]
}
```
{% endcode %}

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: An array of log objects, block hashes, or transaction hashes, depending on filter type.

### Use Case

The eth\_getFilterChanges method is commonly used for:

* **Event polling**
* **Real-time updates**
* **Filter-based monitoring**

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
