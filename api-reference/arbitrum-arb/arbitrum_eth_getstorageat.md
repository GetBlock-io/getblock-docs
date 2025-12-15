---
description: >-
  Example code for the eth_getStorageAt JSON RPC method. Сomplete guide on how
  to use eth_getStorageAt JSON RPC in GetBlock Web3 documentation.
---

# eth\_getStorageAt - Arbitrum

This method returns the value stored at a specific storage slot of a contract. Every smart contract stores its data in 32-byte slots, and this method provides direct low-level access to them.

#### Parameters

| Parameter     | Type         | Required | Description                                                                                    |
| ------------- | ------------ | -------- | ---------------------------------------------------------------------------------------------- |
| address       | string       | yes      | The target contract address. Must be a valid Ethereum-style address.                           |
| position      | string (hex) | yes      | The storage slot index. Must be a 32-byte hex value (e.g. `"0x0"`, `"0x1"`, `"0xabc..."`).     |
| block\_number | string       | yes      | Block identifier such as `"latest"`, `"earliest"`, `"pending"`, or a hex-encoded block number. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
       "method": "eth_getStorageAt",
    "params": [
        "0x80d40e32FAD8bE8da5C6A42B8aF1E181984D137c",
        "0x0",
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
       "method": "eth_getStorageAt",
    "params": [
        "0x80d40e32FAD8bE8da5C6A42B8aF1E181984D137c",
        "0x0",
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
        "method": "eth_getStorageAt",
    "params": [
        "0x80d40e32FAD8bE8da5C6A42B8aF1E181984D137c",
        "0x0",
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
    "method": "eth_getStorageAt",
    "params": [
        "0x80d40e32FAD8bE8da5C6A42B8aF1E181984D137c",
        "0x0",
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
    "result": "0x000000000000000000000000aadc76c6e4abeeb52d764443357f6e75db28f0e1"
}
```

#### Reponse Parameter Definition

| Field  | Type         | Description                                                                     |
| ------ | ------------ | ------------------------------------------------------------------------------- |
| result | string (hex) | 32-byte hex value stored at the given slot. Empty slots return `"0x000...000"`. |

#### Use case

The `eth_getStorageAt` is used to:

* Reading contract variables without calling a function
* Inspecting mappings, arrays, and structs by computing their storage slots
* Powering dashboards and analytics tools that pull historical state data
* Helping wallets validate allowance, balances, or approvals stored on-chain
* Supporting audits by letting developers inspect raw contract storage

#### Error handling

| Status Code | Error Message    | Cause                                                                                                                                                                |
| ----------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.                                                                                                                                    |
| -32602      | Invalid argument | <ul><li>The Address not 20 bytes or missing prefix</li><li>Slot value not valid hex</li><li>Block number incorrect or out of range</li><li>keyword missing</li></ul> |

#### Integration with Web3

The `eth_getStorageAt` method helps developers to:

* Build trustless analytics dashboards
* Inspect contract internals without a function call
* Recover lost data values during debugging
* Read live smart contract state with zero gas
* Simulate contract reads on historical blocks
* Validate approvals or configurations stored in private mappings

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getStorageAt",
    [
        "0x80d40e32FAD8bE8da5C6A42B8aF1E181984D137c",
        "0x0",
        "latest"
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
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'),
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
        method: "eth_getStorageAt",
        params: [
        "0x80d40e32FAD8bE8da5C6A42B8aF1E181984D137c",
        "0x0",
        "latest"
    ]
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
