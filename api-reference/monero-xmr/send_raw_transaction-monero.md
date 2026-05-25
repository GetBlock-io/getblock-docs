# send\_raw\_transaction monero

This non-JSON-RPC endpoint broadcasts a serialized signed transaction to the network. It is the primary submission point for transactions constructed and signed offline by a wallet.

## Parameters

| Parameter      | Type    | Required | Description                                                                            |
| -------------- | ------- | -------- | -------------------------------------------------------------------------------------- |
| tx\_as\_hex    | string  | Yes      | Hex-encoded signed transaction blob                                                    |
| do\_not\_relay | boolean | No       | If `true`, the node will accept the tx but not propagate it (rare; useful for testing) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/send_raw_transaction' \
--header 'Content-Type: application/json' \
--data-raw '{
    "tx_as_hex": "de6a3\u2026",
    "do_not_relay": false
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "tx_as_hex": "de6a3\u2026",
    "do_not_relay": false
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/send_raw_transaction',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/send_raw_transaction"

payload = json.dumps({
    "tx_as_hex": "de6a3\u2026",
    "do_not_relay": false
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
        "tx_as_hex": "de6a3\u2026",
        "do_not_relay": false
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/send_raw_transaction")
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
    "double_spend": false,
    "fee_too_low": false,
    "invalid_input": false,
    "invalid_output": false,
    "low_mixin": false,
    "not_relayed": false,
    "overspend": false,
    "reason": "",
    "status": "OK",
    "too_big": false,
    "too_few_outputs": false,
    "untrusted": false
}
```
{% endcode %}

## Response Parameters

| Field           | Type    | Description                                                    |
| --------------- | ------- | -------------------------------------------------------------- |
| status          | string  | `OK` if the transaction was accepted; otherwise a failure code |
| reason          | string  | Human-readable rejection reason (empty on success)             |
| double\_spend   | boolean | `true` if rejected as a double-spend                           |
| fee\_too\_low   | boolean | `true` if rejected because the fee was below the minimum       |
| low\_mixin      | boolean | `true` if the ring size was below the network minimum          |
| invalid\_input  | boolean | `true` if an input was invalid                                 |
| invalid\_output | boolean | `true` if an output was invalid                                |
| not\_relayed    | boolean | `true` if accepted but not relayed (e.g. `do_not_relay=true`)  |
| overspend       | boolean | `true` if outputs exceed inputs                                |
| too\_big        | boolean | `true` if the transaction exceeds the maximum allowed size     |

## Use Cases

* Broadcasting signed XMR transactions from cold wallets
* Mining-pool payouts and operational disbursements
* Exchanges and merchant settlement flows

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
// Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
// Use a plain HTTP client as shown in the Request Example above.
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/send_raw_transaction', {"tx_as_hex": "de6a3\u2026", "do_not_relay": false});
console.log(response.data);
```
{% endtab %}

{% tab title="monero-python" %}
```python
# Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
# Use a plain HTTP client as shown in the Request Example above.
import requests;

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/send_raw_transaction', json={"tx_as_hex": "de6a3\u2026", "do_not_relay": false})
print(response.json())
```
{% endtab %}
{% endtabs %}
