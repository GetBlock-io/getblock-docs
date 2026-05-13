---
description: >-
  Example code for the eth_getBlockByHash json-rpc method. Сomplete guide on how
  to use eth_getBlockByHash json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - Optimism

This method retrieves comprehensive block data using the block's unique hash identifier. It returns information including the block number, parent hash, miner address (sequencer on Optimism), state root, transactions, gas used, and timestamps. This is useful when you have a block hash from a transaction receipt or event log and need to retrieve the full block details.

### Parameters

| Parameter        | Type           | Description                                                                           | Required |
| ---------------- | -------------- | ------------------------------------------------------------------------------------- | -------- |
| blockHash        | DATA, 32 Bytes | The hash of a block.                                                                  | Yes      |
| fullTransactions | Boolean        | If true, returns full transaction objects; if false, returns only transaction hashes. | Yes      |

### Request Sample

{% tabs %}
{% tab title="curl" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": ["0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e", false],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Javascript(Axios)" %}
{% code overflow="wrap" %}
```js
import axios from 'axios'
let data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": [
        "0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e",
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
    "method": "eth_getBlockByHash",
    "params": [
        "0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e",
        false
    ],
    "id": "getblock.io"
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
v
```
{% endcode %}
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
    "method": "eth_getBlockByHash",
    "params": [
        "0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e",
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
        "baseFeePerGas": "0x186",
        "blobGasUsed": "0x337bb0",
        "difficulty": "0x0",
        "excessBlobGas": "0x0",
        "extraData": "0x01000000fa000000020000000000000000",
        "gasLimit": "0x2625a00",
        "gasUsed": "0xc091f0",
        "hash": "0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e",
        "logsBloom": "0x4872328010200000000118820a104000088120ca700030009a84060048c37b00050810003004000000400090250031026044014004022600060801604024004100000000004c20ca2028200943a850a20c848212010c00b0285243002400000134000040020508403000004109001d0001020002080da4041140c11208881c0c002405030800040060210200800804000104948005210000000000562e80001082824000984000000080000004b000002000004000d80008009f0140804008c4011018020830000124009e0c004a008098c00510218003084000800022206400051c0e0900b410002840000a020823202080088040c4088804024000000108a0",
        "miner": "0x4200000000000000000000000000000000000011",
        "mixHash": "0xb7e417a24558626d905d7ba4244a530feb5dfe3c1133a6f2a40042592edbf6cb",
        "nonce": "0x0000000000000000",
        "number": "0x9083a84",
        "parentBeaconBlockRoot": "0x12a26f129e4b1094abe3c40313df0bcff65f2271b8927797eb72fb67a4bf79b2",
        "parentHash": "0xf9aac23d07a7983d7a89eb6d8142bdfafbf310aef860d6bea7d26def28820545",
        "receiptsRoot": "0x68900de2a05f1b3c98d8494114ab16907c9e5545b95272eb5194f990d0626419",
        "requestsHash": "0xe3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x42c2",
        "stateRoot": "0xac5b500a4a48312cfc55d1e219eaf7c315e23a41132b90aed2cea01c21963b9d",
        "timestamp": "0x6a044ec1",
        "transactions": [
            "0x8932181eb716327425152d53ee6401d2e4a7a9a478253725a8fe4b74e4d04585",
            "0xde98b7d698fb75a4cc24145eb0c993fc6c812bb94c18525e392373f42762bd00",
            "0xeb1a4ea3ac7f00321c6fcb536cb7a23969fc075dab91729dcd190abfc4e9ce8c",
            "0xcee2f556d6c664467b5947f2c767b8bf4f4670de1e5acacff36d492443272c5d",
            "0xfd33982711b9f7741aa479dc70adf5b399467ba3483bd081c7471f0205f32bec",
            "0x7d5396bcf3e70d688ad9ec0386469b67740b1b4f7ead31c9fce164cdd6e806f6",
            "0xa8ae9a59540aa48bb8653265957f135bf66ae6f780ea0b32e1192862e0781e00",
            "0xd4f53ee06df0d976f15ea62ac2119770d6d3633ebbcf7f0eefefc366b3d56642",
            "0xd5ec97eaa2b70ecec68edb4143e9b4cf4017631d574655dc1ff7340e727761a5",
            "0x628317fb2812a7aa1180cba629b3cb34fe006054d46dc005f23505760a9cff34",
            "0xc7edb92d918b49dc59a8f425b84bcb0b01d00d78419b7aedcd56529496b61291",
            "0xea5ad34c6e72149a233e1f7de47e16b6038d83b7e4f41f4a2cd55346ade06afc",
            "0x5df9e8c2510c2c266b5cbbf1176e5f36b7cad5ff41bf92f1df8c5c84a3ddfa14",
            "0xf96b8da9249f3c91a5e7fac55b863cab30f1895869a36e1baa7eb1e6c77731dd",
            "0x8f0b4759a0efea2f871814f5e46be0887ba2c63f0b9e08bfe4a095d334bfe374",
            "0x8b5b2c89e369ce1b75b53ad2e1f70166f45770c932847fdb3b80aabdc8e17acf",
            "0xe14bec7504049d1aeaf9393135bd09223e39a8eef24c6decdc2c4930cd956d68",
            "0x29d024cb8a8953b61c43477eee304e93b4b360ad216168eee4e7e897c0c6a9ee",
            "0x24da31d56d29dbbbf22a2f7017f53c001c81cef4ca8d265d00f98e6b7aa47bd7",
            "0xe0980a79f0ea9c878909e88ea07a28317a1e91e431b0ee1a71510174fcb5f4f7",
            "0xcfe77ef71e25ce18eae7057b0ee1bf417c07d717b40203dc7e938103bec1fbee",
            "0x69d2d1f9f6f3d90392d739189bfbcfe31d4a70fd829a72c88c0a6f3b40e8f9c5",
            "0xc6c6c97167f3bf936f056d340e9ad4ad59e316e3b519d4dd53f3a1305c3bce91",
            "0xc96b1683c65a1d0f9ca424a064e320340ecb9f19d1325a956f0e7d4d7d6487df",
            "0x841be33e1143041bb41058f16d9adaf7754428fd8ffb91f7dd421ca1170c7e67",
            "0x8c68f5500b9655f9bb6bf5b5d96b0c2176403ba955f5159d569585aa0a5ce07e",
            "0x990ff496d46228da332d6157cf8fc9a25d04384ed2e3682edd4499bf99e9c218",
            "0xfd908142ab27051fc5d3281df48c74b9666058ef2049a9cee428fb8195fad7f6",
            "0x9eeec6dba1ee4d4694c23d64ea3316c9b052bbd3e659e54cb6eec25e84d7deb1",
            "0x03267d56dd916fdf44b303233c422c26ed462e61853319d80cacf29153ee0ef0",
            "0xdd3ef0deaec7f36076d7025bc22aad918909537fac7cb60d21955e44dde510c5",
            "0x2006efcb307b96a8d5afcd541990d35052533229a7ae3b60e16578bf62768214",
            "0x1adc546e3023b3f5b4e24bb8bf0e7d34c2ec80eff8146bda0041aa12edfaf66c",
            "0xf0150f585b7b872c9d562d1937622fbd12524185bf2d898e0b0848c13e5e629a",
            "0x7dffdbc55446d44f33b0ca6295c71367a2ce63d75c54cbe757ec1d9f3afb9160",
            "0xdfe7cc23e2bdd3708a621a354500e2e220a62cac0184ddf49275534caf011e32",
            "0xe8008427f5a3cd3c09a61ec5a5440ca2a8ccdfbc5e8d663a32f93495f4064589",
            "0x3350f9a0f3cce702228850f2e7e0d2be80700c6adf4ff96ce29f56f9062faa62",
            "0x35a840c41d580fd4c7abcb674b9d2047dbb97fd6667f14dbfda41cfe05ef0a2f",
            "0x1feb374f76bcd9bf22fd34681c0f6c52089e941d546f4ebb0badb1743298cc31"
        ],
        "transactionsRoot": "0xa56fffcc8384271957700fa4023a4a4b7b412596ed52b6522ecc7e3e0ff1e047",
        "uncles": [],
        "withdrawals": [],
        "withdrawalsRoot": "0xebb725766294094628aad41b7ed015b5b9e246018e22a07b9a4c5418f8c19fd0"
    }
}

```
{% endcode %}

### Response Parameters

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

### Use Case

The eth\_getBlockByHash method is commonly used for:

* **Block verification**
* **Transaction confirmation**
* **Chain reorganization detection**
* **Historical data retrieval**

#### Error handling

| Status Code | Error Message    | Cause                                                                               |
| ----------- | ---------------- | ----------------------------------------------------------------------------------- |
| 404         | Not Found        | Missing or invalid ACCESS\_TOKEN.                                                   |
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
  "0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e",
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
            method: 'eth_getBlockByHash',
            params: [ "0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e",
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
{% endcode %}
{% endtab %}
{% endtabs %}
