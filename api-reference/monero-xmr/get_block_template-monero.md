---
description: >-
  Example code for the get_block_template JSON-RPC method. Complete guide on how
  to use get_block_template JSON-RPC in GetBlock Web3 documentation.
---

# get\_block\_template - Monero

This method returns a block blob and accompanying data that miners use to construct and submit a new block. The block reward is paid to the supplied wallet address.

## Parameters

| Parameter       | Type         | Required | Description                                                                        |
| --------------- | ------------ | -------- | ---------------------------------------------------------------------------------- |
| wallet\_address | string       | Yes      | Address of the wallet that will receive the coinbase reward if the block is mined  |
| reserve\_size   | unsigned int | Yes      | Reserve size for extra bytes in the coinbase (often used by pools for nonce space) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_block_template",
    "params": {
        "wallet_address": "44GBHzv6ZyQdJkjqZje6KLZ3xSyN1hBSFAnLP6EAqJtCRVzMzZmeXTC2AHKDS9aEDTRKmo6a6o9r9j86pYfhCWDkKjbtcns",
        "reserve_size": 60
    },
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "get_block_template",
    "params": {
        "wallet_address": "44GBHzv6ZyQdJkjqZje6KLZ3xSyN1hBSFAnLP6EAqJtCRVzMzZmeXTC2AHKDS9aEDTRKmo6a6o9r9j86pYfhCWDkKjbtcns",
        "reserve_size": 60
    },
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
    "method": "get_block_template",
    "params": {
        "wallet_address": "44GBHzv6ZyQdJkjqZje6KLZ3xSyN1hBSFAnLP6EAqJtCRVzMzZmeXTC2AHKDS9aEDTRKmo6a6o9r9j86pYfhCWDkKjbtcns",
        "reserve_size": 60
    },
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
        "method": "get_block_template",
        "params": {
                "wallet_address": "44GBHzv6ZyQdJkjqZje6KLZ3xSyN1hBSFAnLP6EAqJtCRVzMzZmeXTC2AHKDS9aEDTRKmo6a6o9r9j86pYfhCWDkKjbtcns",
                "reserve_size": 60
        },
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
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockhashing_blob": "0e0eb6cdacf405fa7bdb281e0153d84cd55a3fcaa57c3e570f4a49f935850b5e3000000000aac4...",
        "blocktemplate_blob": "0e0eb6cdacf405fa7bdb281e0153d84cd55a3fcaa57c3e570f4a49f935850b5e3000000000aac4...",
        "difficulty": 294558564173,
        "expected_reward": 600000000000,
        "height": 912345,
        "prev_hash": "37155cae7d3aa8ca7dc8b9e69e0d1f64b5e3a8b1c2d9e4f5a6b7c8d9e0f1a2b3",
        "reserved_offset": 130,
        "status": "OK",
        "untrusted": false
    }
}
```
{% endcode %}

## Response Parameters

| Field                      | Type         | Description                                                     |
| -------------------------- | ------------ | --------------------------------------------------------------- |
| result.blockhashing\_blob  | string       | Hex blob to feed into the proof-of-work hash function           |
| result.blocktemplate\_blob | string       | Hex blob of the full block template                             |
| result.difficulty          | unsigned int | Current network difficulty                                      |
| result.expected\_reward    | unsigned int | Expected coinbase reward in atomic units                        |
| result.height              | unsigned int | Height the block would be added at                              |
| result.prev\_hash          | string       | Hash of the previous block                                      |
| result.reserved\_offset    | unsigned int | Byte offset within the block where the reserve bytes are placed |

## Use Cases

* Mining pool block construction
* Solo mining client integration
* Testing mining software end-to-end

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
import monerojs from 'monero-javascript';

const daemon = await monerojs.connectToDaemonRpc('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the daemon client. For raw access:
const result = await daemon.getDaemonConnection().sendJsonRequest('get_block_template', {"wallet_address": "44GBHzv6ZyQdJkjqZje6KLZ3xSyN1hBSFAnLP6EAqJtCRVzMzZmeXTC2AHKDS9aEDTRKmo6a6o9r9j86pYfhCWDkKjbtcns", "reserve_size": 60});
console.log(result);
```
{% endtab %}

{% tab title="monero-python" %}
```python
from monero.backends.jsonrpc import JSONRPCDaemon
from monero.daemon import Daemon

backend = JSONRPCDaemon(host='go.getblock.io', port=443, path='/<ACCESS-TOKEN>/', protocol='https')
daemon = Daemon(backend)

# For methods covered by the typed API, use the daemon attribute directly.
# For raw RPC access:
result = backend.raw_request('get_block_template', {"wallet_address": "44GBHzv6ZyQdJkjqZje6KLZ3xSyN1hBSFAnLP6EAqJtCRVzMzZmeXTC2AHKDS9aEDTRKmo6a6o9r9j86pYfhCWDkKjbtcns", "reserve_size": 60})
print(result)
```
{% endtab %}
{% endtabs %}
