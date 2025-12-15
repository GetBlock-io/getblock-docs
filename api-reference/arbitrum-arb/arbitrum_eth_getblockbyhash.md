---
description: >-
  Example code for the eth_getBlockByHash JSON RPC method. Сomplete guide on how
  to use eth_getBlockByHash JSON RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByHash - Arbitrum

This method retrieves a full block using its block hash. Optionally includes full transaction objects or just the transaction hashes.

#### Parameters

| Parameter          | Type    | Required | Description                                                                                   |
| ------------------ | ------- | -------- | --------------------------------------------------------------------------------------------- |
| block\_hash        | string  | yes      | The 32-byte block hash to query (0x-prefixed).                                                |
| full\_transactions | boolean | yes      | If **true**, returns full transaction objects. If **false**, returns only transaction hashes. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": [
        "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
        false
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
    "method": "eth_getBlockByHash",
    "params": [
        "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
        false
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
    "method": "eth_getBlockByHash",
    "params": [
        "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
        false
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
    "method": "eth_getBlockByHash",
    "params": [
        "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
        false
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
    "result": {
        "baseFeePerGas": "0x5f5e100",
        "difficulty": "0x1",
        "extraData": "0xe31298ca6df96cb00d355ca9554878c19d39292b6a26eb9fc54b5df482851ef7",
        "gasLimit": "0x4000000000000",
        "gasUsed": "0x229dd8",
        "hash": "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
        "l1BlockNumber": "0x105e046",
        "logsBloom": "0x00000100000000020000100000000000080000000000000000000000001000001000000000000000000000000101000000000000000000000001010000000000000010000000000000001008000120000100000000001000800100008000000000000000000000000000000000000000021000200000400000000010000000001000000000000000000000000008000000000001000000000008000000000010000000000000000000000000000000000000000000000000000000000000000000008002000048000000000000000000200800000000000010000000000000000000000040000000000000008000000010000000004010400000000000000040",
        "miner": "0xa4b000000000000000000073657175656e636572",
        "mixHash": "0x0000000000012069000000000105e046000000000000000a0000000000000000",
        "nonce": "0x00000000000c7662",
        "number": "0x5206d53",
        "parentHash": "0xb458d44871af788992e51a89661eb28a6539cf9f33c0e9a30d0aab5f2e50e974",
        "receiptsRoot": "0x184ea05480454e4c4fcf6406106a4cb13477af97d45752a126ee84de88695c7c",
        "sendCount": "0x12069",
        "sendRoot": "0xe31298ca6df96cb00d355ca9554878c19d39292b6a26eb9fc54b5df482851ef7",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x579",
        "stateRoot": "0xea41be04084dc613cfbf8d1fef75d9a097fad2b222373669e42e2aa173175649",
        "timestamp": "0x644f0242",
        "transactions": [
            "0xaf256a9ce8839489dcaa4f9a2740bcd5865b3db34a15434cce29e6bdcf043bc7",
            "0xfd7e27e19777598be2536bc6e4a5e89cc8a386eca46374333b1fba22641ed255",
            "0x98b2f04b2eb747e685c7db6f97ef2aaca236e146afb310ed039d56ab8599eb0e",
            "0x23c566cf1c2f208a74ef2e585b5bfe7fcffab6445027f06b4207ec43968cf71e",
            "0x9febdfb50d48f1891b6660379dcea3d0060808538a102d589e7ea3b7aa6d787d"
        ],
        "transactionsRoot": "0x22ef4159c9394a555af55a3ae5719bf15c1ff296838dd920f1566eeecb15aa1f",
        "uncles": []
    }
}
```

#### Reponse Parameter Definition

| Field            | Type         | Description                                  |
| ---------------- | ------------ | -------------------------------------------- |
| number           | string (hex) | Block number                                 |
| hash             | string       | Block hash                                   |
| parentHash       | string       | Previous block hash                          |
| nonce            | string       | 64-bit hash used for PoW (Arbitrum always 0) |
| sha3Uncles       | string       | Keccak hash of uncles                        |
| logsBloom        | string       | Bloom filter for logs                        |
| transactionsRoot | string       | Root of transaction trie                     |
| stateRoot        | string       | Root of state trie                           |
| miner            | string       | Validator / proposer address                 |
| difficulty       | string (hex) | Arbitrum always zero                         |
| totalDifficulty  | string (hex) | Always zero                                  |
| size             | string (hex) | Size of the block in bytes                   |
| gasLimit         | string (hex) | Gas limit for the block                      |
| gasUsed          | string (hex) | Gas consumed                                 |
| timestamp        | string (hex) | Unix timestamp                               |
| transactions     | array        | Either full objects or transaction hashes    |
| uncles           | array        | Always empty for Arbitrum                    |

#### Use case

The `eth_getBlockHash` is used to:

* Fetching block details for explorers and dashboards
* Tracking new blocks in real time
* Verifying a transaction’s block inclusion
* Monitoring gas usage per block
* Analyzing contract activity in a specific block
* Auditing validator performance through block metadata

#### Error handling

| Status Code | Error Message    | Cause                                                                               |
| ----------- | ---------------- | ----------------------------------------------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.                                                   |
| -32602      | Invalid argument | The block hash isn't accurate or incomplete or full\_transaction boolean is missing |

#### Integration with Web3

The `eth_getBlockByHash` can help developers:

* Observing full historical block data
* Powering explorers with detailed transaction info
* Displaying block metadata in analytics dashboards
* Reconstructing block timelines for auditing and security tools
* Connecting validator/rollup behavior to specific blocks

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getBlockByHash", [
  "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
  false,
]);    console.log("The result:", result);
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
            method: 'eth_getBlockByHash',
            params: [ "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
 false],
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
