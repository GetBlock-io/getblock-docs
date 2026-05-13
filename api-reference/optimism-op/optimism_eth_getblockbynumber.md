---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - Optimism

This method retrieves comprehensive block data, including the block hash, parent hash, miner address, state root, transactions, gas used, and timestamps. You can request either full transaction objects or just transaction hashes. You can also use block tags like `'latest'`, `'earliest'`, or `'pending'`. This is fundamental for block explorers, analytics tools, and applications that need to process block data.

### Parameters

| Parameter        | Type          | Description                                                                           | Required |
| ---------------- | ------------- | ------------------------------------------------------------------------------------- | -------- |
| blockNumber      | QUANTITY\|TAG | Block number in hex, or `'latest'`, `'earliest'`, `'pending'`.                        | Yes      |
| fullTransactions | Boolean       | If true, returns full transaction objects; if false, returns only transaction hashes. | Yes      |

### Request Sample

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["latest", false],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Javascript(axios)" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
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
{% endcode %}
{% endtab %}

{% tab title="Python(Request)" %}
{% code overflow="wrap" %}
```py
import requests
import json

url = "https://go.getblock.us/<ACESS_TOKEN>"

payload = json.dumps({
   "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
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
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::builder()
        .build()?;

    let mut headers = reqwest::header::HeaderMap::new();
    headers.insert("Content-Type", "application/json".parse()?);

    let data = r#"{
   "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
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
    "result": {
        "baseFeePerGas": "0x181",
        "blobGasUsed": "0x208e60",
        "difficulty": "0x0",
        "excessBlobGas": "0x0",
        "extraData": "0x01000000fa000000020000000000000000",
        "gasLimit": "0x2625a00",
        "gasUsed": "0x8588fa",
        "hash": "0x99e5fc28e1416cadb3e29d4b82982e962b3fcf6f0e27ba51010b641a4bd326cd",
        "logsBloom": "0x404022800000008000011002420000000000208ad00000001004020040c30b00010810403000000000080090840030024444808000004002460000004064004100000000004400422020200802083016000000020804081000000000200000011400004012010000200000410908180000840000080440040101c0100040000000040002080000000025000080080400000084000c2100000000000002000014020040000840000100000000002000002000000000d000080003014080400080001008020020400100001a00000000809880001000820008400000002100641004150c02019410002000000000002020208a0880000000000400000000000800",
        "miner": "0x4200000000000000000000000000000000000011",
        "mixHash": "0x678947991d0f7b67c46e217401db8da3fe123ab553a352e6cc3041caa9d8aae5",
        "nonce": "0x0000000000000000",
        "number": "0x9083bfd",
        "parentBeaconBlockRoot": "0x449d214c35c68304a2005942bd495624e8d9dfb7e3a548424c57e57f577a98d0",
        "parentHash": "0x3b5c3cc394b13eecae8250726265dab0f4d0341b850578cb650f2f02575d99c3",
        "receiptsRoot": "0xd2675e8ce5eb4040e866821a9190548432a2d0391c8f0db911872bcb47e7c297",
        "requestsHash": "0xe3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x2827",
        "stateRoot": "0x3226dec3d13bc605c905378c6e2cf10ab0ee33839fb7801e72f6fcf0605ec52c",
        "timestamp": "0x6a0451b3",
        "transactions": [
            "0xca621c835a0d7864396582548cb66efeb78bd1eee5fc257a85dc578915c23ea8",
            "0x262db5ae0abc67b5d7e1f1bc5f19fde6fee91e1e7ef5d4ac8bc03796703c4335",
            "0x8d212ac370a806c28a68cb7ff2d20e327c0b79884b901d8bc8dc138095ae757b",
            "0xd1c55cc6c0242a445dc9022504c6ca898aacadd31b9daaa2fd069871f264beab",
            "0x2903bd2b58c9ace4eb1b548ab716b37c1f8ad0e2b6ccfeb9f2ea21928d55da7e",
            "0xd1d4bff18b1ec9f82fef3bc81defd05e2a293d0657788ac15a3cf5f88ab32feb",
            "0x9769c6cd163f24ad1ae830abf61d8f2f53a2ed10073eaed6f4b8a79c7f3f30e7",
            "0x47d79c1702035a7317ae095903a8149690de85fc0de8f208cfa4467ede019e83",
            "0xacbf028f58483849e5daaa94829b4c4b9211372aa3b88e4929f311fe38b5473b",
            "0x704762fc6bea3d39f74c97466420dce759a3f757698ffd9c732b1717fd8f984f",
            "0x58437d9342fc6cea8bd649f71e57612783a549478890debde8ec44df0a45f684",
            "0x45ce73800598a4ce04b1a9bfa9b63b90d2bfaf5fdca8ea6170e081ecc39d28f4",
            "0x5d763f4e12f3b4c5a73010ffbffe67219c655a2be39d44cb7db9b8c3228b7966",
            "0x4fa2b0e76a0b2333bbf39d23dc278dc0b03eee94649433b1fa011927b65f258a",
            "0x1b62aa8c695355d7f0c293a7d3e1dd7e4b246e2c571727671a74ee9e2d8c8e9b",
            "0xe8ae8decb13a30b13df33bd6bb89673ca814c4deccb672b72dc664112e18b267",
            "0xe6f191704056b5df64ec572e28a4ee45542b11c9c899dcbb639968b1d49a479b",
            "0x494d1fec627a7d0682b536f3f33570dfb26b5d00fe4b59c46505151fba5ed869",
            "0xef80d3247ce40b22c77d8a86496c37edaa3629549d473e3e6e4288f6d833f18a",
            "0xe210d90176aaa2fe4288b0b62b7fb6c91ea31bdffd7fe178f396abe8a3d4804d",
            "0x87da697c937a04ce6601de7352c0f0d373bfa77f13ccced1276da6583f44a647",
            "0xfa2c58bb9773559d98ac27ddcdd70c0efcdb1135f512fa44e428be6ade10cd82",
            "0x967e6e720c27878decdd537929eb1224059ae6cbc1e0dd272a6eb649d4b40543",
            "0xe8d90c875fb7364b62fa48650728c2076e3fa226904d8619ee74ad82c562dfa1",
            "0x5c78f8d54bad21bd91bb544cb18ee654fbfc9a38087a692d6bcb02aefdcf54c7"
        ],
        "transactionsRoot": "0xfe575f7cf7696cdb34f68b39ccb0e5716fe8f71dd69c601410d1a58cb9fb51a2",
        "uncles": [],
        "withdrawals": [],
        "withdrawalsRoot": "0xc6d7dab5fee8bef51be63af837d26e482fd88f993fb64d7238b7f1a4e2ecccc4"
    }
}

```
{% endcode %}

### Response Parameters

| Field            | Type         | Description                           |
| ---------------- | ------------ | ------------------------------------- |
| number           | string (hex) | Block number                          |
| hash             | string       | Block hash                            |
| parentHash       | string       | Previous block hash                   |
| nonce            | string       | Always 0 on Arbitrum                  |
| sha3Uncles       | string       | Keccak hash of uncles                 |
| logsBloom        | string       | Bloom filter for logs                 |
| transactionsRoot | string       | Root of transactions trie             |
| stateRoot        | string       | Root of state trie                    |
| miner            | string       | Address that proposed the block       |
| difficulty       | string       | Always zero on Arbitrum               |
| totalDifficulty  | string       | Always zero                           |
| size             | string       | Block size in bytes                   |
| gasLimit         | string       | Block gas limit                       |
| gasUsed          | string       | Total gas consumed                    |
| timestamp        | string       | Unix timestamp                        |
| transactions     | array        | List of transaction hashes or objects |
| uncles           | array        | Always empty                          |

### Use Case

The eth\_getBlockByNumber method is commonly used for:

* **Block exploration**
* **Transaction history**
* **Chain analysis**
* **Data indexing**

#### Error handling

| Status Code | Error Message    | Cause                                                                               |
| ----------- | ---------------- | ----------------------------------------------------------------------------------- |
| 404         | Not Found        | Missing or invalid ACCESS\_TOKEN.                                                   |
| -32602      | Invalid argument | The block hash isn't accurate or incomplete or full\_transaction boolean is missing |

#### Integration with Web3

The `eth_getBlockByNumber` can help developers:

* Stream blocks in real time for analytics
* Power live dashboards with block updates
* Confirm block finality before executing contracts
* Inspect transaction activity between specific blocks
* Support trustless frontends that monitor chain state

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getBlockByNumber", ["latest", false],);    console.log("The result:", result);
    return result;
  } catch (error) {
    console.error("The error:", error);
    throw error;
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
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
            method: 'eth_getBlockByNumber',
            params: ["latest", false],
        });
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }
}

```
{% endcode %}
{% endtab %}
{% endtabs %}
