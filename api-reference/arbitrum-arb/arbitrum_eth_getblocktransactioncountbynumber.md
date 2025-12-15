---
description: >-
  Example code for the eth_getBlockTransactionCountByNumber JSON RPC method.
  Сomplete guide on how to use eth_getBlockTransactionCountByNumber JSON RPC in
  GetBlock Web3 documentation.
---

# eth\_getBlockTransactionCountByNumber - Arbitrum

This method returns the number of transactions in a block identified by its block number.

#### Parameters

| Parameter     | Type   | Required | Description                                                                          |
| ------------- | ------ | -------- | ------------------------------------------------------------------------------------ |
| block\_number | string | yes      | Block number in hex format, or a keyword such as `latest`, `earliest`, or `pending`. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByNumber ",
    "params": [
        "latest"
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
   "method": "eth_getBlockTransactionCountByNumber",
    "params": [
        "latest"
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
    "method": "eth_getBlockTransactionCountByNumber",
    "params": [
        "latest"
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
   "method": "eth_getBlockTransactionCountByNumber",
    "params": [
        "latest"
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
    "result": "0x8"
}
```

#### Reponse Parameter Definition

| Field  | Type         | Description                                                                              |
| ------ | ------------ | ---------------------------------------------------------------------------------------- |
| result | string (hex) | Number of transactions in the specified block. Returns null if the block does not exist. |

#### Use case

The `eth_getBlockTransactionCountByNumber` is used to:

* Calculating chain throughput by measuring transactions per block
* Building analytics dashboards that monitor block activity in real time
* Lightweight block scanning without fetching full block data
* Detecting unusually busy or empty blocks
* Supporting explorers and monitoring tools that show activity patterns

#### Error handling

| Status Code | Error Message    | Cause                                                          |
| ----------- | ---------------- | -------------------------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.                              |
| -32602      | Invalid argument | The block hash isn't accurate or incomplete or keyword missing |

#### Integration with Web3

The `eth_getBlockTransactionCountByNumber`  can help developers:

* Track live activity on the chain
* Build explorers and dashboards without fetching heavy block objects
* Detect transaction spikes for alerts and monitoring
* Estimate network load to optimize dapp behavior
* Streamline backend indexing by filtering blocks based on transaction count

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getBlockTransactionCountByNumber",
["latest"]);    
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
           method:   "eth_getBlockTransactionCountByNumber",
<strong>           params:   ["latest"]
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
