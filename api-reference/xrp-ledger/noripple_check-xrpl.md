---
description: >-
  Example code for the noripple_check JSON RPC method. Complete guide to using
  noripple_check JSON-RPC in the GetBlock Web3 documentation.
---

# noripple\_check - XRPL

This method provides a quick way to check the status of the NoRipple flag on an account's trust lines, which may be useful for token issuers.

## Parameters

| Parameter     | Type          | Required | Description           |
| ------------- | ------------- | -------- | --------------------- |
| account       | string        | Yes      | Account to check      |
| role          | string        | Yes      | "gateway" or "user"   |
| transactions  | boolean       | No       | Include suggested txs |
| limit         | number        | No       | Maximum problems      |
| ledger\_hash  | string        | No       | Ledger hash           |
| ledger\_index | string/number | No       | Ledger index          |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "noripple_check",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "role": "gateway",
        "transactions": true
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="noripple_check.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'noripple_check',
    params: [{
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        role: 'gateway',
        transactions: true
    }],
    id: 'getblock.io'
};

axios.post(url, payload, { headers })
    .then(response => console.log(response.data))
    .catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="noripple_check.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "noripple_check",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "role": "gateway",
        "transactions": True
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="noripple_check.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "noripple_check",
        "params": [{
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "role": "gateway",
            "transactions": true
        }],
        "id": "getblock.io"
    });

    let response = client
        .post("https://xrp.getblock.io/mainnet/")
        .header("x-api-key", "YOUR-API-KEY")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "result": {
        "ledger_current_index": 63632030,
        "problems": [],
        "status": "success"
    }
}
```

## Response Parameters

| Parameter    | Type  | Description     |
| ------------ | ----- | --------------- |
| problems     | array | Issues found    |
| transactions | array | Suggested fixes |

## Use Cases

{% hint style="info" %}
* Gateway compliance
* Trust line auditing
* Token issuer checks
{% endhint %}

## Error Handling

<details>

<summary>actNotFound</summary>

Account not found

</details>

<details>

<summary>invalidParams</summary>

Invalid parameters

</details>

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl-noripple-check.js" %}
```javascript
const xrpl = require('xrpl');

async function norippleCheck() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'noripple_check',
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        role: 'gateway'
    });
    
    console.log('Problems:', response.result.problems);
    await client.disconnect();
}

norippleCheck();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-noripple-check.py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import NoRippleCheck

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = NoRippleCheck(
    account="rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
    role="gateway"
)
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
