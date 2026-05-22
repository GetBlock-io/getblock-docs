# zks\_estimategasl1tol2 zksync

Estimates the gas required for an L1 to L2 transaction. Use this when preparing L1 contracts that send messages or assets into zkSync Era.

## Parameters

| Parameter   | Type   | Required | Description                                                              |
| ----------- | ------ | -------- | ------------------------------------------------------------------------ |
| callRequest | object | Yes      | Transaction-shaped object with `from`, `to`, `data`, and optional fields |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_estimateGasL1ToL2",
    "params": [
        {
            "from": "0x1111111111111111111111111111111111111111",
            "to": "0x2222222222222222222222222222222222222222",
            "data": "0xffffffff"
        }
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
    "method": "zks_estimateGasL1ToL2",
    "params": [
        {
            "from": "0x1111111111111111111111111111111111111111",
            "to": "0x2222222222222222222222222222222222222222",
            "data": "0xffffffff"
        }
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
    "method": "zks_estimateGasL1ToL2",
    "params": [
        {
            "from": "0x1111111111111111111111111111111111111111",
            "to": "0x2222222222222222222222222222222222222222",
            "data": "0xffffffff"
        }
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
        "method": "zks_estimateGasL1ToL2",
        "params": [
                {
                        "from": "0x1111111111111111111111111111111111111111",
                        "to": "0x2222222222222222222222222222222222222222",
                        "data": "0xffffffff"
                }
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
    "result": "0x25f64db"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                    |
| ------ | ------ | ---------------------------------------------- |
| result | string | Estimated gas amount in hexadecimal (32 bytes) |

## Use Cases

* Bridging and L1→L2 message senders
* Setting correct gas budgets in L1 deposit contracts
* Bridge UX showing accurate cost estimates

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('zks_estimateGasL1ToL2', [{"from": "0x1111111111111111111111111111111111111111", "to": "0x2222222222222222222222222222222222222222", "data": "0xffffffff"}]);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

// zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'zks_estimateGasL1ToL2'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('zks_estimateGasL1ToL2', [{"from": "0x1111111111111111111111111111111111111111", "to": "0x2222222222222222222222222222222222222222", "data": "0xffffffff"}])
print(result)
```
{% endtab %}
{% endtabs %}
