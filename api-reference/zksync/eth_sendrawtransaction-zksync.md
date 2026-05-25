---
description: >-
  Example code for the eth_sendRawTransaction JSON-RPC method. Сomplete guide on
  how to use eth_sendRawTransaction JSON-RPC in GetBlock.io Web3 documentation.
---

# eth\_sendRawTransaction - zkSync

Broadcasts a signed transaction to zkSync Era for execution. The primary method for submitting any state-changing operation. zkSync supports standard EIP-1559 (type 2) and EIP-712 (type 113, for advanced features like paymasters).

## Parameters

| Parameter    | Type   | Required | Description                                                                                               |
| ------------ | ------ | -------- | --------------------------------------------------------------------------------------------------------- |
| signedTxData | string | Yes      | The signed transaction data (hex), produced offline by a signer such as zksync-ethers, ethers.js, or viem |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0x02f86c8201448504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0x02f86c8201448504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0x02f86c8201448504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_sendRawTransaction",
        "params": [
                "0x02f86c8201448504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
        ],
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

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                               |
| ------ | ------ | ------------------------------------------------------------------------- |
| result | string | Transaction hash, used to track inclusion via `eth_getTransactionReceipt` |

## Use Cases

* Send ETH transfers on L2
* Execute ERC-20 token transfers
* Interact with DeFi protocols on zkSync Era
* Deploy smart contracts
* Submit EIP-712 transactions with paymaster sponsorship

## Error Handling

| Status Code | Error Message                       | Cause                                                                         |
| ----------- | ----------------------------------- | ----------------------------------------------------------------------------- |
| 403         | Forbidden                           | Missing or invalid `<ACCESS-TOKEN>`                                           |
| -32602      | Invalid params                      | Request parameters are missing or malformed                                   |
| 429         | Too Many Requests                   | Rate limit exceeded for your plan                                             |
| -32000      | Insufficient funds for gas          | Sender does not have enough ETH to pay gas + value (unless using a paymaster) |
| -32000      | Nonce too low                       | Transaction's nonce has already been used                                     |
| -32000      | Replacement transaction underpriced | Replacement transaction must offer a higher gas price                         |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
```javascript
import { Provider, Wallet } from 'zksync-ethers';
import { ethers } from 'ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');
const wallet = new Wallet(privateKey, provider);

const tx = await wallet.sendTransaction({
    to: '0xd85498dbeaeb1df24be52eed4f52eac2fbd56245',
    value: ethers.parseEther('0.1')
});
console.log('Tx Hash:', tx.hash);
await tx.wait();
console.log('Confirmed!');
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder
from zksync2.signer.eth_signer import PrivateKeyEthSigner
from zksync2.core.types import Token
from eth_account import Account

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')
account = Account.from_key(PRIVATE_KEY)
signer = PrivateKeyEthSigner(account, zk_web3.zksync.chain_id)

# Build, sign, and send a transaction
# (See zksync2-python docs for full TxFunctionCall / TxCreateContract examples)
```
{% endtab %}
{% endtabs %}
