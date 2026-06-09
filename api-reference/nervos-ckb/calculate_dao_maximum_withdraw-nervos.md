---
description: >-
  Example code for the calculate_dao_maximum_withdraw JSON-RPC method. Complete
  guide on how to use calculate_dao_maximum_withdraw JSON-RPC in GetBlock Web3
  documentation.
---

# calculate\_dao\_maximum\_withdraw - Nervos

Calculates the maximum CKB amount that a Nervos DAO deposit cell can withdraw. Nervos DAO is a protocol-level mechanism for locking CKB to receive secondary issuance — this method computes the matured value given the deposit's out point and a target withdraw header.

## Parameters

| Parameter  | Type                          | Required | Description                                                           |
| ---------- | ----------------------------- | -------- | --------------------------------------------------------------------- |
| out\_point | OutPoint                      | Yes      | Out point of the Nervos DAO deposit cell                              |
| kind       | DaoWithdrawingCalculationKind | Yes      | Either a withdraw block hash (`H256`) or a withdraw header (`Header`) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "calculate_dao_maximum_withdraw",
    "params": [
        {
            "tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
            "index": "0x0"
        },
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "calculate_dao_maximum_withdraw",
    "params": [
        {
            "tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
            "index": "0x0"
        },
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"
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
    "method": "calculate_dao_maximum_withdraw",
    "params": [
        {
            "tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
            "index": "0x0"
        },
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"
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
        "method": "calculate_dao_maximum_withdraw",
        "params": [
                {
                        "tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
                        "index": "0x0"
                },
                "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"
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
    "result": "0xe8d4a51a00"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                      |
| ------ | ------ | ------------------------------------------------ |
| result | Uint64 | Maximum withdrawable CKB amount in Shannon (hex) |

## Use Cases

* Nervos DAO portfolio dashboards showing accumulated rewards
* Wallet UIs for DAO deposit/withdraw flows
* Computing optimal withdraw timing for DAO depositors

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('calculate_dao_maximum_withdraw', [{"tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0", "index": "0x0"}, "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"]);
console.log(result);
```
{% endtab %}

{% tab title="ckb-sdk-python / requests" %}
```python
# Using the official CKB SDK Python (ckb-sdk-python).
# For methods not yet wrapped, fall back to raw requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'calculate_dao_maximum_withdraw',
    'params': [{"tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0", "index": "0x0"}, "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
