---
description: >-
  Example code for the eth_getBlockTransactionCountByHash JSON RPC method.
  Сomplete guide on how to use eth_getBlockTransactionCountByHash JSON RPC in
  GetBlock Web3 documentation.
---

# eth\_getBlockTransactionCountByHash - Arbitrum

This method returns the number of transactions contained in a block identified by its block hash.

#### Parameters

| Parameter   | Type   | Required | Description                                                                                                      |
| ----------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------- |
| block\_hash | string | yes      | Hash of the block whose transaction count is being requested. Must be a valid 32-byte hex string with 0x prefix. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByHash",
    "params": [
        "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a"
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
   "method": "eth_getBlockTransactionCountByHash",
    "params": [
        "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a"
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
    "method": "eth_getBlockTransactionCountByHash",
    "params": [
        "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a"
    ],
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
   "method": "eth_getBlockTransactionCountByHash",
    "params": [
        "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a"
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

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x5"
}
```

#### Reponse Parameter Definition

| Field  | Type         | Description                                                             |
| ------ | ------------ | ----------------------------------------------------------------------- |
| result | string (hex) | Transaction count of the block. Returns null if the block is not found. |

#### Use case

The `eth_getBlockTransactionCountByHash` is used to:

* Counting total transactions in a given block
* Building block explorers or analytics dashboards
* Monitoring throughput or spikes in chain activity
* Identifying empty or low-activity blocks
* Pre-processing blocks before downloading the full transaction list
* Reducing bandwidth when only counts are needed instead of full block data

#### Error handling

| Status Code | Error Message    | Cause                                        |
| ----------- | ---------------- | -------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.            |
| -32602      | Invalid argument | The block hash isn't accurate or incomplete  |

#### Integration with Web3

The `eth_getBlockTransactionCountByHash` can help developers:

* Power block explorers that show transaction counts without loading full blocks
* Measure chain activity for dashboards and metrics
* Build fast monitoring tools that track throughput
* Reduce bandwidth usage by avoiding heavy block queries
* Support systems that decide whether to fetch full block data based on count

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getBlockTransactionCountByHash",
["0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a"]);    
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
<pre class="language-jsx"><code class="lang-jsx">import { createPublicClient, http } from 'viem';
import { arbitrum } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: arbitrum,
  transport: http('https://go.getblock.us/&#x3C;ACCESS_TOKEN>'),
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
           method:   "eth_getBlockTransactionCountByHash",
<strong>           params:   ["0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a"]
</strong>        });
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }
}

</code></pre>
{% endtab %}
{% endtabs %}
