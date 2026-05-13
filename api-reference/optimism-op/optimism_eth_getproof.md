---
description: >-
  Example code for the eth_getProof json-rpc method. Сomplete guide on how to
  use eth_getProof json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getProof - Optimism

This method returns the account information (balance, nonce, codeHash, storageHash) along with Merkle proofs that can be used to verify the data against the state root. It also returns proofs for specified storage keys. This is essential for light clients and cross-chain verification, enabling trustless verification of account and storage state.

### Parameters

| Parameter   | Type           | Description                                              | Required |
| ----------- | -------------- | -------------------------------------------------------- | -------- |
| address     | DATA, 20 Bytes | The address to get proof for.                            | Yes      |
| storageKeys | Array          | Array of storage keys to get proofs for.                 | Yes      |
| block       | QUANTITY\|TAG  | Block number in hex, or 'latest', 'earliest', 'pending'. | Yes      |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{{
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", ["0x0"], "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", ["0x0"], "latest"],
    "id": "getblock.io"
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(JSON.stringify(response.data, null, 2)))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", ["0x0"], "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", ["0x0"], "latest"],
    "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
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
        "address": "0x742d35cc6634c0532925a3b844bc9e7595f7bd5e",
        "accountProof": [
            "0xf90211a0afb41b9ac6c2ee10ff07a462e4a794d09e28f3a1953e405f2b390026792d6f77a02cdeab24b3635e24d28ffe2c1c1498839577df892d78f75c3271ffe1299c73b1a0c63c4706c4eb603a093364d99a00a2936a6c35005d7f1c4cf4da4f746e79dc59a0847fcdf9d5c7373a3ee9306c426ee6bb6584c93349200504e910c0d57486e229a015d327cdc30a32c50ee47aae1d03380a0094259e2f38eef7f20bd214f1ddd93fa0bfea7e107d7a0cd8b922060ec399ccad36a20b15e093f348da00342087ac4bc6a0cdf2c0e9594eb51431e82f6205104fb3e952bf0c54c2cfefc7e840132582b9c8a068442cde0a7ab7f5d526e3ad6cffcd8ba138847171fd1a07aa24ee7cd07d6f85a0fc15f3b5d661e38c810f68ec4d3142b1540f47a8c85b4920ea152ae195e76cc3a05037efcd101adc13d1f18f90eb84be77aeb9f46204a29e70fb1120331a6d0c60a0129332960e79c2dc3952335cf363b6d830b9725f4053423fc835718063724170a0ab9df0b309cdf370bbdec78339d75ec49f5d12538b1086b423ed72f784bb75e7a0a2985792fefa802634ff1153fe4143df09d62db94aa42dbe766c86556e7f7e3aa0bb98f3c0ec620c8b91cbc0a80da86b208b792609d920962073ec391fe63ac012a0f2c39a3f74fc81dd089cbd824a5579026f5250a290216998915018bae4d1ccdfa00937a8969f394fe1a98217d01751c74c7a52950696777f0e79606d6fa4cc1b8b80",
            "0xf85180808080808080808080a0f1c1786be147bcfc55d2ce9a0447087249480a4d097e05d255af0bbc9e60d08280a00ffcd244e6c654b187c01a2adc4b55f85747c38b520107321f7298adddb4c49580808080"
        ],
        "balance": "0x0",
        "codeHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0",
        "storageHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "storageProof": [
            {
                "key": "0x0",
                "value": "0x0",
                "proof": []
            }
        ]
    }
}

```
{% endcode %}

### Response Parameters

| Field        | Type   | Description          |
| ------------ | ------ | -------------------- |
| accountProof | array  | Array of proof nodes |
| balance      | string | Account balance      |
| codeHash     | string | Code hash            |
| nonce        | string | Account nonce        |
| storageHash  | string | Storage root hash    |
| storageProof | array  | Storage proofs       |

### Use Case

The eth\_getProof method is commonly used for:

* **Light client verification**
* **Cross-chain proofs**
* **State verification**
* **Trustless data access**

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const proof = await provider.send('eth_getProof',
  ["0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", ["0x0"], "latest"]);
console.log(proof);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

const client = createPublicClient({
    chain: optimism,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const proof = await client.getProof({
    address: '0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e',
    storageKeys: ['0x0'],
    latest
});
console.log(proof);
```
{% endtab %}
{% endtabs %}
