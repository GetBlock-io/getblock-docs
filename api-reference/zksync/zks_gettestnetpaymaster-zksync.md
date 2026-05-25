---
description: >-
  Example code for the zks_getTestnetPaymaster JSON-RPC method. Сomplete guide
  on how to use zks_getTestnetPaymaster JSON-RPC in GetBlock.io Web3
  documentation.
---

# zks\_getTestnetPaymaster - zkSync

Returns the address of the testnet paymaster contract. The testnet paymaster sponsors gas for any transaction, making testing easier. **Testnet only** — returns `null` on mainnet.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getTestnetPaymaster",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "zks_getTestnetPaymaster",
    "params": [],
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
    "method": "zks_getTestnetPaymaster",
    "params": [],
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
        "method": "zks_getTestnetPaymaster",
        "params": [],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x3cb2b87d10ac01736a65688f3e0fb1b070b3eea3"
}
```

## Response Parameters

| Field  | Type           | Description                                     |
| ------ | -------------- | ----------------------------------------------- |
| result | string \| null | Testnet paymaster address, or `null` on mainnet |

## Use Cases

* Testnet development with gas sponsorship
* Onboarding flows that sponsor first transactions
* Account abstraction examples and demos

## Error Handling

| Status Code | Error Message     | Cause                               |
| ----------- | ----------------- | ----------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>` |
| 429         | Too Many Requests | Rate limit exceeded for your plan   |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('zks_getTestnetPaymaster', []);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'zks_getTestnetPaymaster'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('zks_getTestnetPaymaster', [])
print(result)
```
{% endtab %}
{% endtabs %}
