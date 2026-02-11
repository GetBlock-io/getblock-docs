---
description: >-
  Example code for the ledger_entry JSON RPC method. Complete guide to using
  ledger_entry JSON-RPC in the GetBlock Web3 documentation.
---

# ledger\_entry - XRPL

This method returns a single ledger entry from the XRP Ledger in its raw format.

## Parameters

| Parameter        | Type          | Required | Description              |
| ---------------- | ------------- | -------- | ------------------------ |
| index            | string        | No       | Object ID                |
| account\_root    | string        | No       | Account address          |
| directory        | object        | No       | Directory specification  |
| offer            | object        | No       | Offer specification      |
| ripple\_state    | object        | No       | Trust line specification |
| check            | string        | No       | Check ID                 |
| escrow           | object        | No       | Escrow specification     |
| payment\_channel | string        | No       | Channel ID               |
| deposit\_preauth | object        | No       | Preauth specification    |
| ticket           | object        | No       | Ticket specification     |
| ledger\_hash     | string        | No       | Ledger hash              |
| ledger\_index    | string/number | No       | Ledger index             |

## Request Examples

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "ledger_entry",
    "params": [{
        "account_root": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "ledger_index": "validated"
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
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
    method: 'ledger_entry',
    params: [{
        account_root: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        ledger_index: 'validated'
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
    "method": "ledger_entry",
    "params": [{
        "account_root": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "ledger_index": "validated"
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
        "method": "ledger_entry",
        "params": [{
            "account_root": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "ledger_index": "validated"
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

{% code title="response.json" %}
```json
{
    "result": {
        "index": "B1CB040A17F9469BC00376EC8719535655824AD16CB5F539DD5765FEA88FDBE3",
        "ledger_current_index": 102053097,
        "node": {
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "Balance": "1070051282",
            "Flags": 0,
            "LedgerEntryType": "AccountRoot",
            "OwnerCount": 0,
            "PreviousTxnID": "8CA6DE7FCB677A4E59D34B782AF47298E8426E4C1F5774CD0F0185664C68CE52",
            "PreviousTxnLgrSeq": 100788827,
            "Sequence": 45,
            "index": "B1CB040A17F9469BC00376EC8719535655824AD16CB5F539DD5765FEA88FDBE3"
        },
        "status": "success",
        "validated": false
    }
}
```
{% endcode %}

## Response Parameters

| Parameter     | Type    | Description       |
| ------------- | ------- | ----------------- |
| index         | string  | Object identifier |
| node          | object  | Entry data        |
| ledger\_index | integer | Ledger index      |

## Use Cases

* Get specific entry
* Check account state
* Verify offers/escrows

## Error Handling

| Error Code    | Description        |
| ------------- | ------------------ |
| entryNotFound | Entry not found    |
| invalidParams | Invalid parameters |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js example" %}
```javascript
const xrpl = require('xrpl');

async function getEntry() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'ledger_entry',
        account_root: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH'
    });
    
    console.log('Entry:', response.result.node);
    await client.disconnect();
}

getEntry();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py example" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import LedgerEntry

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = LedgerEntry(account_root="rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH")
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
