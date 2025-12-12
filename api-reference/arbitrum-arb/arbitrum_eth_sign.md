---
description: >-
  Example code for the eth_sign json-rpc method. Сomplete guide on how to use
  eth_sign json-rpc in GetBlock.io Web3 documentation.
---

# eth\_sign- Arbitrum

This method signs an arbitrary message using the private key of the given address.\
The message is prefixed following the Ethereum signed message standard before hashing and signing.

{% hint style="info" %}
This method requires the account to be **unlocked** on the node or handled by a wallet provider such as MetaMask.
{% endhint %}

#### Parameters

| Field   | Type   | Description                                                                          |
| ------- | ------ | ------------------------------------------------------------------------------------ |
| address | string | The account address that signs the message. Must be unlocked or handled by a wallet. |
| message | string | The raw message to sign (hex encoded or UTF-8 encoded).                              |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "eth_sign",
  "params": [
    "0x742d35Cc6634C0532925a3b844Bc454e4438f44e",
    "0x48656c6c6f20576f726c64"
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
    "method": "eth_sign",
  "params": [
    "0x742d35Cc6634C0532925a3b844Bc454e4438f44e",
    "0x48656c6c6f20576f726c64"
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
    "method": "eth_sign",
  "params": [
    "0x742d35Cc6634C0532925a3b844Bc454e4438f44e",
    "0x48656c6c6f20576f726c64"
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
    "method": "eth_sign",
  "params": [
    "0x742d35Cc6634C0532925a3b844Bc454e4438f44e",
    "0x48656c6c6f20576f726c64"
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
  "result": "0xbdfdd41c9ad20871e3ec6e9e...c73f338fd1b1c7784cf9c"
}
```

#### Reponse Parameter Definition

| Field  | Description                                                     | Data Type |
| ------ | --------------------------------------------------------------- | --------- |
| result | The hash of the submitted transaction, as a hexadecimal string. | String    |

#### Use case

The `eth_sign` method helps developers to:

* Verify a user identity without sending a transaction
* Implement login flows for dApps (Sign-in with Ethereum style)
* Generate signatures for permit, authentication, and meta transactions
* Build trustless systems where only message signatures (not transactions) are needed

#### Error handling

| Status Code | Error Message   | Cause                                                                                                         |
| ----------- | --------------- | ------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS\_TOKEN.                                                                             |
| -3200       | Unknown account | <ul><li>Invalid account address</li><li>user rejected request</li><li>message must be hex or string</li></ul> |

#### Integration with Web3

The `eth_sign` method enables developers to:

* Implement secure message signing with intuitive UX
* Build login or authorization flows for any dApp
* Create contract interactions that rely on signed messages
* Validate signed payloads on servers or smart contracts
* Use MetaMask or WalletConnect to sign and broadcast actions securely

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
