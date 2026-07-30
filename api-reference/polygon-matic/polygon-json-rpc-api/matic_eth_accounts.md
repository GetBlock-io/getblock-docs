---
description: >-
  Example code for the eth_accounts json-rpc method. Сomplete guide on how to
  use eth_accounts json-rpc in GetBlock.io Web3 documentation.
---

# eth\_accounts - Polygon

The **eth\_accounts** method returns a list of addresses owned by the client node. Since GetBlock provides access to shared infrastructure rather than personal nodes, this method typically returns an empty array, as account management is not handled by the RPC endpoint.

This method is part of the Polygon JSON-RPC Core API and is useful for checking if any accounts are available for signing transactions directly through the node.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_accounts",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_accounts',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log('Accounts:', response.data.result))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python (requests)" %}
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_accounts",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust (reqwest)" %}
{% code title="main.rs" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "eth_accounts",
            "params": [],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": []
}
```

## Response Parameters

| Field   | Type   | Description                                                  |
| ------- | ------ | ------------------------------------------------------------ |
| jsonrpc | string | JSON-RPC version (2.0)                                       |
| id      | string | Request identifier                                           |
| result  | array  | List of account addresses (typically empty for shared nodes) |

## Use Case

The eth\_accounts method is typically used for:

* Checking if the connected node manages any accounts
* Verifying node configuration for account-based operations
* Determining available signers for transaction signing
* Development and debugging purposes

{% hint style="info" %}
Note: When using GetBlock's shared infrastructure, this method will return an empty array since account private keys are not stored on the shared nodes. For transaction signing, use external wallet solutions or the eth\_sendRawTransaction method with pre-signed transactions.
{% endhint %}

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const accounts = await provider.send('eth_accounts', []);
console.log('Accounts:', accounts);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const accounts = await client.request({
    method: 'eth_accounts',
    params: []
});
console.log('Accounts:', accounts);
```
{% endcode %}
{% endtab %}
{% endtabs %}
