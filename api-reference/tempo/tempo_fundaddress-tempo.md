---
description: >-
  Example code for the tempo_fundAddress JSON-RPC method. Complete guide on how
  to use tempo_fundAddress JSON-RPC in GetBlock Web3 documentation.
---

# tempo\_fundAddress - Tempo

Mints test stablecoins to the given address. On the public Moderato testnet, this currently mints pathUSD, AlphaUSD, BetaUSD, and ThetaUSD. Available only on faucet-enabled testnet endpoints.

## Parameters

{% hint style="info" %}
**TESTNET-FAUCET-ONLY METHOD.** This method is only available on faucet-enabled testnet endpoints (such as the public Moderato testnet at `https://rpc.moderato.tempo.xyz`). It is **not** available on Tempo mainnet. Calling this method against a mainnet endpoint returns an error.
{% endhint %}

| Parameter | Type   | Required | Description                       |
| --------- | ------ | -------- | --------------------------------- |
| address   | string | Yes      | Recipient 20-byte address (0x...) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "tempo_fundAddress",
    "params": [
        "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245"
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
    "method": "tempo_fundAddress",
    "params": [
        "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245"
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
    "method": "tempo_fundAddress",
    "params": [
        "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245"
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
        "method": "tempo_fundAddress",
        "params": [
                "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245"
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
    "result": [
        "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
        "0xabcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890",
        "0xfedcba0987654321fedcba0987654321fedcba0987654321fedcba0987654321",
        "0x9876543210abcdef9876543210abcdef9876543210abcdef9876543210abcdef"
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type            | Description                                                                                            |
| ------ | --------------- | ------------------------------------------------------------------------------------------------------ |
| result | array of string | Array of transaction hashes, one per token minted (typically 4 — pathUSD, AlphaUSD, BetaUSD, ThetaUSD) |

## Use Cases

* Bootstrapping testnet wallets with all the standard test stablecoins in one call
* Onboarding flows in testnet developer tools
* CI/CD pipelines that need funded test accounts

## Error Handling

| Status Code | Error Message     | Cause                                                                               |
| ----------- | ----------------- | ----------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                                 |
| -32602      | Invalid params    | Request parameters are missing or malformed                                         |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                   |
| -32601      | Method not found  | Faucet is not enabled on this endpoint (mainnet endpoints always return this error) |

## SDK Integration

{% tabs %}
{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'tempo_fundAddress',
    params: ["0xd85498dbeaeb1df24be52eed4f52eac2fbd56245"]
}});
console.log(response.data);
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'tempo_fundAddress',
    'params': ["0xd85498dbeaeb1df24be52eed4f52eac2fbd56245"]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
