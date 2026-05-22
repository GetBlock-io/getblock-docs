# unstable\_sendrawtransactionwithdetailedoutput zksync

Executes a transaction and returns its hash along with the storage logs and events that _would_ be generated if the transaction were included in a block. Behavior is similar to `eth_sendRawTransaction` but with extra optimistic output for instant UI updates without waiting for confirmation. **Previously named `zks_sendRawTransactionWithDetailedOutput`** — migrated to the `unstable_*` namespace.

## Parameters

| Parameter | Type   | Required | Description                  |
| --------- | ------ | -------- | ---------------------------- |
| data      | string | Yes      | The signed transaction (hex) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "unstable_sendRawTransactionWithDetailedOutput",
    "params": [
        "0x02f86c8201448504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
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
    "method": "unstable_sendRawTransactionWithDetailedOutput",
    "params": [
        "0x02f86c8201448504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
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
    "method": "unstable_sendRawTransactionWithDetailedOutput",
    "params": [
        "0x02f86c8201448504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
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
        "method": "unstable_sendRawTransactionWithDetailedOutput",
        "params": [
                "0x02f86c8201448504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "transactionHash": "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9",
        "storageLogs": [
            {
                "address": "0x000000000000000000000000000000000000800b",
                "key": "0x7",
                "writtenValue": "0x40000000000000000000000006641f961"
            }
        ],
        "events": [
            {
                "address": "0x000000000000000000000000000000000000800a",
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x00000000000000000000000036615cf349d7f6344891b1e7ca7c72883f5dc049"
                ],
                "data": "0x00000000000000000000000000000000000000000000000000000b9b031bf400",
                "blockHash": null,
                "blockNumber": null,
                "l1BatchNumber": "0x4",
                "transactionHash": "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9",
                "transactionIndex": "0x0",
                "logIndex": null,
                "transactionLogIndex": null,
                "logType": null,
                "removed": false
            }
        ]
    }
}
```

## Response Parameters

| Field                              | Type            | Description                                   |
| ---------------------------------- | --------------- | --------------------------------------------- |
| result.transactionHash             | string          | Hash of the broadcast transaction             |
| result.storageLogs                 | array           | Storage logs that would result from execution |
| result.storageLogs\[].address      | string          | Contract address whose storage changed        |
| result.storageLogs\[].key          | string          | Storage slot key                              |
| result.storageLogs\[].writtenValue | string          | New value written to the storage slot         |
| result.events                      | array           | Events that would be emitted by execution     |
| result.events\[].address           | string          | Contract address that would emit the event    |
| result.events\[].topics            | array of string | Indexed event topics                          |
| result.events\[].data              | string          | Non-indexed event data (ABI-encoded)          |

## Use Cases

* Optimistic UI updates immediately after broadcast
* dApps that need to display state changes before L2 confirmation
* Reducing perceived latency in trading and gaming interfaces

## Error Handling

| Status Code | Error Message              | Cause                                                |
| ----------- | -------------------------- | ---------------------------------------------------- |
| 403         | Forbidden                  | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params             | Request parameters are missing or malformed          |
| -32601      | Method not found           | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests          | Rate limit exceeded for your plan                    |
| -32000      | Insufficient funds for gas | Sender does not have enough ETH to pay gas + value   |
| -32000      | Nonce too low              | Transaction's nonce has already been used            |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('unstable_sendRawTransactionWithDetailedOutput', ["0x02f86c8201448504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."]);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'unstable_sendRawTransactionWithDetailedOutput'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('unstable_sendRawTransactionWithDetailedOutput', ["0x02f86c8201448504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008025a04f3a1c..."])
print(result)
```
{% endtab %}
{% endtabs %}
