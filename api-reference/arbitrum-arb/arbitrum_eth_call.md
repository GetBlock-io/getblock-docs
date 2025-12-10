---
description: >-
  Example code for the eth_call json-rpc method. Сomplete guide on how to use
  eth_call json-rpc in GetBlock.io Web3 documentation.
---

# eth\_call - Arbitrum

This method executes a read-only smart contract call on the Arbitrum blockchain without creating a transaction.

{% hint style="warning" %}
This method does **not** modify the blockchain state. It only _simulates_ the call and returns the output.
{% endhint %}

#### Parameters

| Field       | Type          | Required | Description                                                                    |
| ----------- | ------------- | -------- | ------------------------------------------------------------------------------ |
| transaction | object        | yes      | Main transaction object containing the call data                               |
| from        | string        | optional | Caller address used for the simulation. Included inside the transaction object |
| to          | string        | yes      | Smart contract address to call                                                 |
| gas         | string (hex)  | optional | Gas limit for the simulated call                                               |
| gasPrice    | string (hex)  | optional | Ignored for simulations but allowed for compatibility                          |
| value       | string (hex)  | optional | Amount of ETH to send with the call (usually zero)                             |
| data        | string        | yes      | Encoded function selector plus parameters                                      |
| block tag   | string or hex | optional | Block context for the simulation. Default is latest                            |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
  "jsonrpc": "2.0",
  "method": "eth_call",
  "id": "getblock.io",
  "params": [
    {
      "to": "0x1F98431c8aD98523631AE4a59f267346ea31F984",
      "data": "0x8da5cb5b"
    },
    "latest"
  ]
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "eth_call",
  "id": "getblock.io",
  "params": [
    {
      "to": "0x1F98431c8aD98523631AE4a59f267346ea31F984",
      "data": "0x8da5cb5b"
    },
    "latest"
  ]
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
  "method": "eth_call",
  "id": "getblock.io",
  "params": [
    {
      "to": "0x1F98431c8aD98523631AE4a59f267346ea31F984",
      "data": "0x8da5cb5b"
    },
    "latest"
  ]
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

    let data = r#"{{
  "jsonrpc": "2.0",
  "method": "eth_call",
  "id": "getblock.io",
  "params": [
    {
      "to": "0x1F98431c8aD98523631AE4a59f267346ea31F984",
      "data": "0x8da5cb5b"
    },
    "latest"
  ]
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
    "result": "0x0000000000000000000000002bad8182c09f50c8318d769245bea52c32be46cd"
}
```

#### Reponse Parameter Definition

| Field    | Data Type | Definition                                                                           |
| -------- | --------- | ------------------------------------------------------------------------------------ |
| `result` | string    | The return value of the executed contract function, encoded as a hexadecimal string. |

#### Use case

`eth_call` is used to:

* Track the latest block height
* Monitor chain progress or finality
* Trigger event-based updates when the block number increases

#### Error handling

| Status Code | Error Message | Cause                             |
| ----------- | ------------- | --------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS\_TOKEN. |

#### Integration with Web3

The `eth_call` can help developers to:

* Enables trustless frontends
* Reads live contract state without gas
* Supports dashboards, DeFi analytics, wallets, NFT explorers
* Let's developers simulate transactions before execution

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/24915a36b17143a891d3b10447615258";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number)
     const result = await provider.send("eth_call", [
       {
         to: "0x1F98431c8aD98523631AE4a59f267346ea31F984",
         data: "0x8da5cb5b",
       },
       "latest",
     ]);
    console.log("The result:", result);
    return result;
  } catch (error) {
    console.error("Ethers Error fetching block number:", error);
    throw error;
  }
}


```
{% endtab %}
{% endtabs %}
