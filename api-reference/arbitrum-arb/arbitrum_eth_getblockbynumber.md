---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - Arbitrum

This method retrieves a full block by its block number. Supports returning either full transaction objects or only transaction hashes.

#### Parameters

| Parameter          | Type    | Required | Description                                                                                                   |
| ------------------ | ------- | -------- | ------------------------------------------------------------------------------------------------------------- |
| block\_number      | string  | yes      | Block number as a hex string (for example "0x10fb78") or special tags: `"latest"`, `"earliest"`, `"pending"`. |
| full\_transactions | boolean | yes      | If **true**, returns full transaction objects. If **false**, returns only transaction hashes.                 |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
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
{% endtab %}

{% tab title="Request" %}
```python
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
{% endtab %}
{% endtabs %}

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "baseFeePerGas": "0x7cf30f8",
        "difficulty": "0x1",
        "extraData": "0x539464419cc7e1235fba3837a56b348ecb8ae3a3ed2f1c8839012038bbb59e32",
        "gasLimit": "0x4000000000000",
        "gasUsed": "0x1560d7",
        "hash": "0x1d77c0b85c1d0a75c19c94dc05868c936dc6d6b537c1846613e97110c90b3f48",
        "l1BlockNumber": "0x16dd9f1",
        "logsBloom": "0x000100c2000c0008800000044000200000000000000000180000010401000800000408020108000800800000000000000100400108082210006000000028000000000001000200080014040800002004000000040280000008004020000020412000000c0002040000001824000000000000040000000000004000100008000a0020800080004010000001000000000800000000000001000080000000000000020000200001000000000020800000000000080000000000000000080000001000402002000000200000000000010008040082001000882042200200008000080210040000200000000020000000200000000000080040000000000000400400",
        "miner": "0xa4b000000000000000000073657175656e636572",
        "mixHash": "0x0000000000025f6000000000016dd9f100000000000000280000000000000000",
        "nonce": "0x0000000000221739",
        "number": "0x185eef3d",
        "parentHash": "0x9c39e9f3fbc4b716cc974f5bec6766c95d23caf0dd89c97da33bc39656643437",
        "receiptsRoot": "0xb51b31ab3fccbb7423ae394c8d5e2039b55476d9df233e8031bf1c7d5620e489",
        "sendCount": "0x25f60",
        "sendRoot": "0x539464419cc7e1235fba3837a56b348ecb8ae3a3ed2f1c8839012038bbb59e32",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0xed9",
        "stateRoot": "0x764448b4dcde129e0630c8f010e8a7c6ebcc446ec4cb2c2bea87d8f3ff8d86d8",
        "timestamp": "0x69384f78",
        "transactions": [
            "0xd31a43fdc91c3aaa6b00537d8788b06b28833ea14d237488ac35d5993ed8634c",
            "0xdf1df3a844340e0ae82485f78754818d8d628ac7236b6bcdbe3946c219301a45",
            "0x4c02aa4e537ff54dbfda1b3ef528bd611cfe88225d071fda4fa57a12801a2c88",
            "0x5c4f37fdf581342bb1e0eed9522feb69e988b5270945ce6128d55914b0a76c8f",
            "0x795fdc0ab98a9ec4f651779dfc055677f7a35596dda5292ab966b4c35c935231",
            "0x93e9d4fd77c2a9f34f80b328cbbe0613d943d376277a3758c0023c9e349becaf",
            "0xe05ee8bbc3a0db68529271b76ddcca2e5ac3c52263d57268293c09606d1bcaa6",
            "0x5f9fe747df5850d935f769be1a3e91102cd5ab73750b14b82f7e0afbba7cb30f",
            "0x7ee97b9dfa06eeec0bfac8bdcfb320888092a735174b5b7b15f0d066b6b85ecf",
            "0x8357a5f4721cec2e7d27caa0c0ae909e4db26f0e001888651f6213430e3489fe",
            "0xbc740ac9a94989b232c9445e475eb1cc94c3ed858a869ce0c43029cb9ed9d836",
            "0x89203f417d97ae57eaa1f73f1e08a11a9ffd73936744233555152d37d38cdb6e"
        ],
        "transactionsRoot": "0xacd0886f4df73a6d36dcbe36a854a162d25d2e2b81d6362b174d01582aeae562",
        "uncles": []
    }
}
```

#### Reponse Parameter Definition

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

#### Use case

The `eth_getBlockByNumber` is used to:

* Real-time monitoring of blockchain activity
* Fetching the latest block information for dashboards
* Analyzing block-level gas usage and activity patterns
* Validating the inclusion of transactions within a specific block
* Supporting DeFi applications that depend on accurate state timing
* Powering NFT explorers and block visualizers

#### Error handling

| Status Code | Error Message    | Cause                                                                               |
| ----------- | ---------------- | ----------------------------------------------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.                                                   |
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
{% endtab %}
{% endtabs %}
