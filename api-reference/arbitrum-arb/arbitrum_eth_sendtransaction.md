---
description: >-
  Example code for the eth_sendTransaction JSON RPC method. Сomplete guide on
  how to use eth_sendTransaction JSON RPC in GetBlock Web3 documentation.
---

# eth\_sendTransaction - Arbitrum

This method sends a new transaction to the network. Unlike `eth_sendRawTransaction`, this method requires the node or connected wallet to manage the private key and sign the transaction on behalf of the user. Used mostly in browser wallets or trusted local nodes.

#### Parameters

| Field                | Type           | Description                                                              |
| -------------------- | -------------- | ------------------------------------------------------------------------ |
| from                 | string         | Address sending the transaction. Must be unlocked on the node or wallet. |
| to                   | string or null | Address receiving the transaction. Null if deploying a contract.         |
| gas                  | string         | Gas limit for execution. Optional.                                       |
| gasPrice             | string         | Gas price for the transaction. Optional.                                 |
| maxPriorityFeePerGas | string         | EIP-1559 priority fee. Optional.                                         |
| maxFeePerGas         | string         | EIP-1559 max total fee. Optional.                                        |
| value                | string         | Amount of ETH to transfer, in wei.                                       |
| data                 | string         | Hex encoded data payload for contract interaction. Optional.             |
| nonce                | string         | Nonce for the transaction. If omitted, the node assigns automatically.   |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "eth_sendTransaction",
    "params": [
        {
            "from": "0xD1AF2dAc4e0a9d1F58B99E2f42Bc0320Ed74a7cd",
            "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
            "gas": "0x76c0",
            "gasPrice": "0x9184e72a000",
            "value": "0x9184e72a",
            "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
        }
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
    "method": "eth_sendTransaction",
    "params": [
        {
            "from": "0xD1AF2dAc4e0a9d1F58B99E2f42Bc0320Ed74a7cd",
            "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
            "gas": "0x76c0",
            "gasPrice": "0x9184e72a000",
            "value": "0x9184e72a",
            "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
        }
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
    "method": "eth_sendTransaction",
    "params": [
        {
            "from": "0xD1AF2dAc4e0a9d1F58B99E2f42Bc0320Ed74a7cd",
            "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
            "gas": "0x76c0",
            "gasPrice": "0x9184e72a000",
            "value": "0x9184e72a",
            "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
        }
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
     "method": "eth_sendTransaction",
    "params": [
        {
            "from": "0xD1AF2dAc4e0a9d1F58B99E2f42Bc0320Ed74a7cd",
            "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
            "gas": "0x76c0",
            "gasPrice": "0x9184e72a000",
            "value": "0x9184e72a",
            "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
        }
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
   "result": "0x4f7d1c3f6def3e6b1b98c3044818f60e5fa588f1d6bb64df776c6fcf51244b91"
}
```

#### Reponse Parameter Definition

| Field  | Description                                                     | Data Type |
| ------ | --------------------------------------------------------------- | --------- |
| result | The hash of the submitted transaction, as a hexadecimal string. | String    |

#### Use case

The `eth_sendTransaction` method helps developers to:

* Initiate transactions directly from wallets without manual signing
* Deploy contracts when using browser wallets like MetaMask
* Allow frontends to request transactions from users without exposing private keys
* Create a trustless UI where signing occurs locally on the user's device
* Simplify interactions for beginners by handling nonce, gas, and signing automatically
* Build dApps that request on-chain operations with minimal backend logic

#### Error handling

| Status Code | Error Message   | Cause                                                                                                                                                          |
| ----------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS\_TOKEN.                                                                                                                              |
| -3200       | Unknown account | <ul><li>Invalid wallet address</li><li>Insufficient funds</li><li>gas too low</li><li>gas price too low</li><li>nonce too low</li><li>invalid sender</li></ul> |

#### Integration with Web3

The `eth_sendTransaction` method enables developers to:

* Trigger wallet popups for signing transactions in dApps
* Build trustless browser interfaces where key management stays entirely client side
* Avoid handling private keys on servers
* Create user friendly transaction flows for DeFi, NFTs, gaming, and marketplaces
* Use MetaMask or WalletConnect to sign and broadcast actions securely

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_sendTransaction",
    [
        {
            "from": "0xD1AF2dAc4e0a9d1F58B99E2f42Bc0320Ed74a7cd",
            "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
            "gas": "0x76c0",
            "gasPrice": "0x9184e72a000",
            "value": "0x9184e72a",
            "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
        }
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
{% endtabs %}
