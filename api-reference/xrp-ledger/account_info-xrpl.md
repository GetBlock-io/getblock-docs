---
description: >-
  Example code for the account_info JSON RPC method. Complete guide to using
  account_info JSON-RPC in the GetBlock Web3 documentation.
---

# account\_info - XRPL

This method retrieves information about an account, its activity, and its XRP balance. All information retrieved is relative to a specific ledger version.

## Parameters

| Parameter     | Type          | Required | Description                  |
| ------------- | ------------- | -------- | ---------------------------- |
| account       | string        | Yes      | Account address              |
| ledger\_hash  | string        | No       | 64-character hex ledger hash |
| ledger\_index | string/number | No       | Ledger index or shortcut     |
| queue         | boolean       | No       | Include queued transactions  |
| signer\_lists | boolean       | No       | Include signer lists         |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "account_info",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "ledger_index": "validated"
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'account_info',
    params: [{
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        ledger_index: 'validated'
    }],
    id: 'getblock.io'
};

axios.post(url, payload, { headers })
    .then(response => console.log(response.data))
    .catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "account_info",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "ledger_index": "validated"
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "account_info",
        "params": [{
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
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
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "result": {
        "account_data": {
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
        "account_flags": {
            "allowTrustLineClawback": false,
            "defaultRipple": false,
            "depositAuth": false,
            "disableMasterKey": false,
            "disallowIncomingCheck": false,
            "disallowIncomingNFTokenOffer": false,
            "disallowIncomingPayChan": false,
            "disallowIncomingTrustline": false,
            "disallowIncomingXRP": false,
            "globalFreeze": false,
            "noFreeze": false,
            "passwordSpent": false,
            "requireAuthorization": false,
            "requireDestinationTag": false
        },
        "ledger_hash": "2F1865AEA9A70D048D491819D76CBB3F1B6FA567B03A7411BACFED0043A11342",
        "ledger_index": 102053337,
        "status": "success",
        "validated": true
    }
}
```
{% endcode %}

## Response Parameters

| Parameter              | Type    | Description                 |
| ---------------------- | ------- | --------------------------- |
| Account                | string  | Account address             |
| Balance                | string  | XRP balance in drops        |
| Flags                  | number  | Account flags               |
| OwnerCount             | number  | Objects owned               |
| Sequence               | number  | Next transaction sequence   |
| account\_data          | object  | Account information object  |
| ledger\_current\_index | integer | Current ledger index        |
| validated              | boolean | Whether ledger is validated |

## Use Cases

* Check XRP balance
* Verify account exists
* Get account sequence for transactions
* Monitor account settings

## Error Handling

| Error Code    | Description        |
| ------------- | ------------------ |
| actNotFound   | Account not found  |
| invalidParams | Invalid parameters |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code overflow="wrap" %}
```javascript
const xrpl = require('xrpl');

async function getAccountInfo() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'account_info',
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        ledger_index: 'validated'
    });
    
    console.log('Balance:', xrpl.dropsToXrp(response.result.account_data.Balance), 'XRP');
    await client.disconnect();
}

getAccountInfo();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import AccountInfo

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")

request = AccountInfo(
    account="rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
    ledger_index="validated"
)

response = client.request(request)
print(response.result)
```
{% endtab %}
{% endtabs %}
