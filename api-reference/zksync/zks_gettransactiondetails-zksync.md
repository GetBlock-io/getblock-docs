# zks\_gettransactiondetails zksync

Returns zkSync-specific details for a transaction, including L1 settlement metadata: commit, prove, and execute transaction hashes on Ethereum mainnet, plus fee details and the initiator address.

## Parameters

| Parameter | Type   | Required | Description                 |
| --------- | ------ | -------- | --------------------------- |
| txHash    | string | Yes      | Transaction hash (32 bytes) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getTransactionDetails",
    "params": [
        "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9"
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
    "method": "zks_getTransactionDetails",
    "params": [
        "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9"
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
    "method": "zks_getTransactionDetails",
    "params": [
        "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9"
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
        "method": "zks_getTransactionDetails",
        "params": [
                "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9"
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
    "result": {
        "isL1Originated": true,
        "status": "verified",
        "fee": "0x0",
        "gasPerPubdata": "0x320",
        "initiatorAddress": "0x87869cb87c4fa78ca278df358e890ff73b42a39e",
        "receivedAt": "2023-03-03T23:52:24.169Z",
        "ethCommitTxHash": "0x3da5b6eda357189c9243c41c5a33b1b2ed0169be172705d74681a25217702772",
        "ethProveTxHash": "0x2f482d3ea163f5be0c2aca7819d0beb80415be1a310e845a2d726fbc4ac54c80",
        "ethExecuteTxHash": "0xdaff5fd7ff91333b161de54534b4bb6a78e5325329959a0863bf0aae2b0fdcc6"
    }
}
```
{% endcode %}

## Response Parameters

| Field                   | Type    | Description                                          |
| ----------------------- | ------- | ---------------------------------------------------- |
| result.isL1Originated   | boolean | Whether the transaction originated on L1             |
| result.status           | string  | Current status — typically `verified` once finalized |
| result.fee              | string  | Transaction fee paid (wei, hex)                      |
| result.gasPerPubdata    | string  | Gas per pubdata unit at execution                    |
| result.initiatorAddress | string  | Address that initiated the transaction               |
| result.receivedAt       | string  | ISO 8601 timestamp when the transaction was received |
| result.ethCommitTxHash  | string  | L1 hash of the commit operation                      |
| result.ethProveTxHash   | string  | L1 hash of the proof submission                      |
| result.ethExecuteTxHash | string  | L1 hash of the execution                             |

## Use Cases

* Transaction explorers showing L1 settlement detail
* Auditing per-transaction settlement timing
* Detecting L1-originated vs L2-originated transactions

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
const result = await provider.send('zks_getTransactionDetails', ["0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9"]);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'zks_getTransactionDetails'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('zks_getTransactionDetails', ["0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9"])
print(result)
```
{% endtab %}
{% endtabs %}
