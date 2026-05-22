# eth\_getlogs zksync

Returns logs matching a given filter. Filters specify block range, address, and indexed topics — efficient retrieval of contract events like ERC-20 transfers.

## Parameters

| Parameter    | Type   | Required | Description                                                                                                 |
| ------------ | ------ | -------- | ----------------------------------------------------------------------------------------------------------- |
| filterObject | object | Yes      | Filter with optional `fromBlock`, `toBlock`, `address` (single or array), `topics` (array of topic filters) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x2249b3",
            "toBlock": "latest",
            "address": "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
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
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x2249b3",
            "toBlock": "latest",
            "address": "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
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
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x2249b3",
            "toBlock": "latest",
            "address": "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
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
        "method": "eth_getLogs",
        "params": [
                {
                        "fromBlock": "0x2249b3",
                        "toBlock": "latest",
                        "address": "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
                        "topics": [
                                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
                        ]
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
    "result": [
        {
            "address": "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
            "blockHash": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
            "blockNumber": "0x2249b7",
            "data": "0x0000000000000000000000000000000000000000000000056bc75e2d63100000",
            "logIndex": "0x0",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x00000000000000000000000036615cf349d7f6344891b1e7ca7c72883f5dc049",
                "0x000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd56245"
            ],
            "transactionHash": "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9",
            "transactionIndex": "0x0",
            "l1BatchNumber": "0x72582"
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field                   | Type            | Description                                                         |
| ----------------------- | --------------- | ------------------------------------------------------------------- |
| result                  | array           | Array of log objects matching the filter                            |
| result\[].address       | string          | Contract address that emitted the log                               |
| result\[].topics        | array of string | Indexed event topics; topic 0 is typically the event signature hash |
| result\[].data          | string          | Non-indexed event data (ABI-encoded)                                |
| result\[].l1BatchNumber | string          | L1 batch number (zkSync extension)                                  |

## Use Cases

* Indexing ERC-20 transfers on zkSync
* Tracking DEX swap events for analytics
* Building real-time event-driven dApps
* Backfilling historical contract activity

## Error Handling

| Status Code | Error Message     | Cause                                                                               |
| ----------- | ----------------- | ----------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                                 |
| -32602      | Invalid params    | Request parameters are missing or malformed                                         |
| -32601      | Method not found  | Method does not exist or is not enabled on this node                                |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                   |
| -32005      | Limit exceeded    | Block range or returned result set exceeds the provider's limit; narrow your filter |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('eth_getLogs', [{"fromBlock": "0x2249b3", "toBlock": "latest", "address": "0x36615cf349d7f6344891b1e7ca7c72883f5dc049", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}]);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'eth_getLogs'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('eth_getLogs', [{"fromBlock": "0x2249b3", "toBlock": "latest", "address": "0x36615cf349d7f6344891b1e7ca7c72883f5dc049", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}])
print(result)
```
{% endtab %}
{% endtabs %}
