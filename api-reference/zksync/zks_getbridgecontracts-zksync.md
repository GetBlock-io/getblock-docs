---
description: >-
  Example code for the zks_getBridgeContracts JSON-RPC method. Сomplete guide on
  how to use zks_getBridgeContracts JSON-RPC in GetBlock.io Web3 documentation.
---

# zks\_getBridgeContracts - zksync

Returns the addresses of canonical bridge contracts on both L1 and L2, including the default ERC-20 bridge and the WETH bridge.

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
    "method": "zks_getBridgeContracts",
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
    "method": "zks_getBridgeContracts",
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
    "method": "zks_getBridgeContracts",
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
        "method": "zks_getBridgeContracts",
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "l1Erc20DefaultBridge": "0x57891966931eb4bb6fb81430e6ce0a03aabde063",
        "l2Erc20DefaultBridge": "0x11f943b2c77b743ab90f4a0ae7d5a4e7fca3e102",
        "l1WethBridge": "0x0000000000000000000000000000000000000000",
        "l2WethBridge": "0x0000000000000000000000000000000000000000"
    }
}
```
{% endcode %}

## Response Parameters

| Field                       | Type   | Description                               |
| --------------------------- | ------ | ----------------------------------------- |
| result.l1Erc20DefaultBridge | string | L1 default ERC-20 bridge contract address |
| result.l2Erc20DefaultBridge | string | L2 default ERC-20 bridge contract address |
| result.l1WethBridge         | string | L1 WETH bridge contract address           |
| result.l2WethBridge         | string | L2 WETH bridge contract address           |

## Use Cases

* Bridge integrations needing canonical bridge addresses
* Wallet UI showing supported bridge paths
* Indexers tracking bridge deposit and withdrawal events

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
const result = await provider.send('zks_getBridgeContracts', []);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'zks_getBridgeContracts'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('zks_getBridgeContracts', [])
print(result)
```
{% endtab %}
{% endtabs %}
