---
description: >-
  Example code for the eth_getCode JSON-RPC method. Сomplete guide on how to use
  eth_getCode JSON-RPC in GetBlock.io Web3 documentation.
---

# eth\_getCode - zkSync

Returns the contract bytecode deployed at a given address. An empty result (`0x`) indicates the address is an EOA, not a contract. Note: on zkSync, deployed bytecode is EraVM bytecode, not standard EVM bytecode — use `zks_getBytecodeByHash` for queries by hash.

## Parameters

| Parameter      | Type   | Required | Description                                                                            |
| -------------- | ------ | -------- | -------------------------------------------------------------------------------------- |
| address        | string | Yes      | 20-byte address of the contract                                                        |
| blockParameter | string | Yes      | Block number in hex, or `"latest"`, `"earliest"`, `"pending"`, `"safe"`, `"finalized"` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": [
        "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
        "latest"
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
    "method": "eth_getCode",
    "params": [
        "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
        "latest"
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
    "method": "eth_getCode",
    "params": [
        "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
        "latest"
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
        "method": "eth_getCode",
        "params": [
                "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
                "latest"
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
    "result": "0x0000008003000400..."
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                               |
| ------ | ------ | ------------------------------------------------------------------------- |
| result | string | Contract bytecode in hex (EraVM format), or `0x` if the address is an EOA |

## Use Cases

* Distinguishing contracts from EOAs
* Verifying contract deployment
* Detecting proxy contracts and their implementations

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
{% code overflow="wrap" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('eth_getCode', ["0x36615cf349d7f6344891b1e7ca7c72883f5dc049", "latest"]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'eth_getCode'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('eth_getCode', ["0x36615cf349d7f6344891b1e7ca7c72883f5dc049", "latest"])
print(result)
```
{% endtab %}
{% endtabs %}
