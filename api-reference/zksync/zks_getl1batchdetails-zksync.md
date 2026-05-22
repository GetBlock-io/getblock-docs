# zks\_getl1batchdetails zksync

Returns full details for an L1 batch, including the commit, prove, and execute transaction hashes on Ethereum mainnet, along with associated timestamps and gas prices.

## Parameters

| Parameter     | Type    | Required | Description                  |
| ------------- | ------- | -------- | ---------------------------- |
| l1BatchNumber | integer | Yes      | The L1 batch number to query |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getL1BatchDetails",
    "params": [
        468355
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
    "method": "zks_getL1BatchDetails",
    "params": [
        468355
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
    "method": "zks_getL1BatchDetails",
    "params": [
        468355
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
        "method": "zks_getL1BatchDetails",
        "params": [
                468355
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
        "number": 468355,
        "timestamp": 1711649164,
        "l1TxCount": 1,
        "l2TxCount": 2363,
        "rootHash": "0x7b31ef880f09238f13b71a0f6bfea340b9c76d01bba0712af6aa0a4f224be167",
        "status": "verified",
        "commitTxHash": "0x5b2598bf1260d498c1c6a05326f7416ef2a602b8a1ac0f75b583cd6e08ae83cb",
        "committedAt": "2024-03-28T18:24:49.713730Z",
        "proveTxHash": "0xc02563331d0a83d634bc4190750e920fc26b57096ec72dd100af2ab037b43912",
        "provenAt": "2024-03-29T03:09:19.634524Z",
        "executeTxHash": "0xbe1ba1fdd17c2421cf2dabe2908fafa26ff4fa2190a7724d16295dd9df72b144",
        "executedAt": "2024-03-29T18:18:04.204270Z",
        "l1GasPrice": 47875552051,
        "l2FairGasPrice": 25000000,
        "baseSystemContractsHashes": {
            "bootloader": "0x010007ede999d096c84553fb514d3d6ca76fbf39789dda76bfeda9f3ae06236e",
            "default_aa": "0x0100055b041eb28aff6e3a6e0f37c31fd053fc9ef142683b05e5f0aee6934066"
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                 | Type    | Description                                             |
| --------------------- | ------- | ------------------------------------------------------- |
| result.number         | integer | L1 batch number (echoed)                                |
| result.timestamp      | integer | Unix timestamp when the batch was processed             |
| result.l1TxCount      | integer | Number of L1 transactions in the batch                  |
| result.l2TxCount      | integer | Number of L2 transactions in the batch                  |
| result.rootHash       | string  | Root hash of the state after processing the batch       |
| result.status         | string  | Status: `committed`, `proven`, or `executed`/`verified` |
| result.commitTxHash   | string  | Ethereum tx hash of the commit operation                |
| result.proveTxHash    | string  | Ethereum tx hash of the proof submission                |
| result.executeTxHash  | string  | Ethereum tx hash of the execution                       |
| result.l1GasPrice     | integer | L1 gas price at the time of batch processing            |
| result.l2FairGasPrice | integer | Fair gas price on L2 at the time of batch processing    |

## Use Cases

* L1-L2 settlement monitoring
* Batch explorers showing commit/prove/execute timeline
* Auditing zkSync's finality cadence

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
const result = await provider.send('zks_getL1BatchDetails', [468355]);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'zks_getL1BatchDetails'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('zks_getL1BatchDetails', [468355])
print(result)
```
{% endtab %}
{% endtabs %}
