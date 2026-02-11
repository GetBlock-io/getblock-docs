---
description: >-
  Example code for the account_lines JSON RPC method. Complete guide to using
  account_lines JSON-RPC in the GetBlock Web3 documentation.
---

# account\_lines - XRPL

This method returns information about an account's trust lines, which track balances of issued currencies.

## Parameters

| Parameter     | Type          | Required | Description              |
| ------------- | ------------- | -------- | ------------------------ |
| account       | string        | Yes      | Account address          |
| peer          | string        | No       | Filter by counterparty   |
| ledger\_hash  | string        | No       | Ledger hash              |
| ledger\_index | string/number | No       | Ledger index or shortcut |
| limit         | number        | No       | Maximum results          |
| marker        | object        | No       | Pagination marker        |

## Request Examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "account_lines",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH"
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'account_lines',
    params: [{
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH'
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
{% code title="example.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "account_lines",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH"
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "account_lines",
        "params": [{
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH"
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
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "ledger_current_index": 63631836,
        "lines": [
            {
                "account": "rrh7rf1gV2pXAoqA8oYbpHd8TKv5ZQeo67",
                "balance": "100",
                "currency": "USD",
                "limit": "1000",
                "limit_peer": "0",
                "no_ripple": true
            }
        ],
        "status": "success"
    }
}
```

## Response Parameters

| Parameter              | Type    | Description           |
| ---------------------- | ------- | --------------------- |
| account                | string  | Counterparty address  |
| balance                | string  | Current balance       |
| currency               | string  | Currency code         |
| limit                  | string  | Trust limit           |
| ledger\_current\_index | integer |  Current ledger index |

## Use Cases

* View token balances
* Check trust line settings
* Monitor issued currencies

## Error Handling

| Error Code    | Description        |
| ------------- | ------------------ |
| actNotFound   | Account not found  |
| invalidParams | Invalid parameters |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js example" %}
```javascript
const xrpl = require('xrpl');

async function getLines() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'account_lines',
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH'
    });
    
    response.result.lines.forEach(line => {
        console.log(`${line.currency}: ${line.balance}`);
    });
    
    await client.disconnect();
}

getLines();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py example" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import AccountLines

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = AccountLines(account="rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH")
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
