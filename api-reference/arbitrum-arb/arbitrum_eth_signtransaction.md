---
description: >-
  Example code for the eth_signTransaction json-rpc method. Сomplete guide on
  how to use eth_signTransaction json-rpc in GetBlock.io Web3 documentation.
---

# eth\_signTransaction - Arbitrum

This method signs a transaction using the private key of the given account **without broadcasting.** This method requires the signing account to be **unlocked** or controlled by a wallet provider.

{% hint style="info" %}
This method requires the account to be **unlocked** on the node or handled by a wallet provider such as MetaMask.
{% endhint %}

#### Parameters

| Field                | Type   | Required | Description                                                                      |
| -------------------- | ------ | -------- | -------------------------------------------------------------------------------- |
| from                 | string | Yes      | The address signing the transaction. Must be unlocked or controlled by a wallet. |
| to                   | string | Yes      | Receiver address. Null if deploying a smart contract.                            |
| gas                  | string | no       | Gas limit for execution.                                                         |
| gasPrice             | string | no       | Gas price in wei.                                                                |
| maxPriorityFeePerGas | string | no       | EIP 1559 priority fee.                                                           |
| maxFeePerGas         | string | Yes      | EIP 1559 max total fee.                                                          |
| value                | string | Yes      | Amount of ETH sent, in wei. .                                                    |
| data                 | string | Yes      | Hex encoded input data for contract interaction. .                               |
| nonce                | string | Yes      | Nonce number for the transaction. .                                              |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "eth_signTransaction",
    "params": [
        {
            "from":  "0xd1af2dac4e0a9d1f58b99e2f42bc0320ed74a7cd",
        
            "to": "0xd26ea0f03100358b2ebd4c9638f042aada9a1bcf",
            "gas": "0x76c0",
            "gasPrice": "0x9184e72a000",
            "nonce": "0x3c",
            "value": "0x9184e72a",
            "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
        }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
    "jsonrpc": "2.0",
   "method": "eth_signTransaction",
    "params": [
        {
            "from":  "0xd1af2dac4e0a9d1f58b99e2f42bc0320ed74a7cd",
        
            "to": "0xd26ea0f03100358b2ebd4c9638f042aada9a1bcf",
            "gas": "0x76c0",
            "gasPrice": "0x9184e72a000",
            "nonce": "0x3c",
            "value": "0x9184e72a",
            "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
}],
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
    "method": "eth_signTransaction",
    "params": [
        {
            "from":  "0xd1af2dac4e0a9d1f58b99e2f42bc0320ed74a7cd",
        
            "to": "0xd26ea0f03100358b2ebd4c9638f042aada9a1bcf",
            "gas": "0x76c0",
            "gasPrice": "0x9184e72a000",
            "nonce": "0x3c",
            "value": "0x9184e72a",
            "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
}],
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
    "method": "eth_sign",
  "method": "eth_signTransaction",
    "params": [
        {
            "from":  "0xd1af2dac4e0a9d1f58b99e2f42bc0320ed74a7cd",
        
            "to": "0xd26ea0f03100358b2ebd4c9638f042aada9a1bcf",
            "gas": "0x76c0",
            "gasPrice": "0x9184e72a000",
            "nonce": "0x3c",
            "value": "0x9184e72a",
            "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
}],
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
  "id": 1,
  "result": {
    "raw": "0xf86c808...",
    "tx": {
      "nonce": "0x0",
      "gas": "0x5208",
      "value": "0x0",
      "to": "0xE592427A0AEce92De3Edee1F18E0157C05861564",
      "from": "0x742d35Cc6634C0532925a3b844Bc454e4438f44e"
    }
  }
}

```

#### Reponse Parameter Definition

| Field                   | Type           | Description                                                                    |
| ----------------------- | -------------- | ------------------------------------------------------------------------------ |
| raw                     | string         | Signed raw transaction in hex. Can be broadcast using eth\_sendRawTransaction. |
| tx.from                 | string         | Address that signed the transaction.                                           |
| tx.to                   | string or null | Recipient address or null for contract deployment.                             |
| tx.nonce                | string         | Nonce used for the transaction.                                                |
| tx.gas                  | string         | Gas limit.                                                                     |
| tx.gasPrice             | string         | Gas price in wei for legacy transactions.                                      |
| tx.maxPriorityFeePerGas | string         | Priority fee (EIP 1559).                                                       |
| tx.maxFeePerGas         | string         | Maximum total fee (EIP 1559).                                                  |
| tx.value                | string         | Amount of ETH sent in wei.                                                     |
| tx.data                 | string         | Encoded call data or contract bytecode.                                        |
| tx.chainId              | string         | Chain identifier for replay protection.                                        |
| tx.v                    | string         | Recovery ID used to verify the signature.                                      |
| tx.r                    | string         | First half of the signature.                                                   |
| tx.s                    | string         | Second half of the signature.                                                  |

#### Use case

The `eth_signTransaction` method helps developers to:

* Prepare transactions before broadcasting them
* Build cold wallet or multisig-style signing workflows
* Use relayers or batch processors that require signed payloads
* Enable advanced transaction flows where signing and sending occur separately
* Allow hardware wallets or custodial services to authorize a transaction before submission
* Support meta transaction systems and gas-sponsored flows

#### Error handling

| Status Code | Error Message   | Cause                                                                                                         |
| ----------- | --------------- | ------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS\_TOKEN.                                                                             |
| -3200       | Unknown account | <ul><li>Invalid account address</li><li>user rejected request</li><li>message must be hex or string</li></ul> |

#### Integration with Web3

The `eth_sign` method enables developers to:

* Sign transactions without broadcasting them
* Deliver signed raw transactions to relayers

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_sign",
 [
    "0x742d35Cc6634C0532925a3b844Bc454e4438f44e",
    "0x48656c6c6f20576f726c64"
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
{% endtabs %}
