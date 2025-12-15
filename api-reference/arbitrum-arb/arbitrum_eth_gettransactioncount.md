---
description: >-
  Example code for the eth_getTransactionCount JSON RPC method. Сomplete guide
  on how to use eth_getTransactionCount JSON RPC in GetBlock Web3 documentation.
---

# eth\_getTransactionCount - Arbitrum

This method returns the number of transactions sent from an address. This value is also known as the nonce. It is required when creating new transactions because each transaction from the same sender must have a unique nonce.

#### Parameters

| Parameter | Type   | Required | Description                                                                                                     |
| --------- | ------ | -------- | --------------------------------------------------------------------------------------------------------------- |
| address   | string | yes      | The address for which to retrieve the transaction count. Must be a hex string with `0x` prefix.                 |
| block     | string | yes      | Block context for the query. Accepted values include `latest`, `earliest`, `pending`, or a block number in hex. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
   "method": "eth_getTransactionCount",
    "params": [
        "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
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
     "method": "eth_getTransactionCount",
    "params": [
        "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
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
    "method": "eth_getTransactionCount",
    "params": [
        "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
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
   "method": "eth_getTransactionCount",
    "params": [
        "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
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
    "result": "0x50"
}
```

#### Reponse Parameter Definition

| Field  | Type         | Description                                                                     |
| ------ | ------------ | ------------------------------------------------------------------------------- |
| result | string (hex) | Number of transactions sent from this address. Equivalent to the account nonce. |

#### Use case

The `eth_getTransactionCount` :

* Determining the correct nonce before sending a new transaction
* Replaying transaction history for analytics or dashboards
* Monitoring the activity of accounts in explorers
* Validating whether a wallet has pending transactions
* Preventing double-spending by ensuring nonces are sequential

#### Error handling

| Status Code | Error Message    | Cause                                                                    |
| ----------- | ---------------- | ------------------------------------------------------------------------ |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.                                        |
| -32602      | Invalid argument | <ul><li>Invalid address</li><li>Block parameter not recognized</li></ul> |

#### Integration with Web3

The `eth_getTransactionCount` method helps developers to:

* Manage nonces automatically when sending transactions
* Prevent duplicate transactions by checking the latest nonce
* Build reliable wallet transaction pipelines
* Track account activity in explorers and dashboards
* Detect pending transactions by comparing `pending` vs `latest` values
* Support batch transaction systems that depend on precise nonce control



{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getTransactionByHash",
    [
        "0xfd11ef35c7179439723e026cb7857ea5a2e48a19257bec00b0ed26672f632181"
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
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'),
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
         method: "eth_getTransactionCount",
      params: ["0xb8b2522480f850eb198ada5c3f31ac528538d2f5", "latest"]});
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
