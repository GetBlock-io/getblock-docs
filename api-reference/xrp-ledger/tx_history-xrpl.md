---
description: >-
  Example code for the tx_history JSON RPC method. Complete guide to using
  tx_history JSON-RPC in the GetBlock Web3 documentation.
---

# tx\_history - XRPL

This method retrieves some of the most recent transactions made. This method is deprecated and may be removed in future versions.

{% hint style="warning" %}
Deprecated: The `tx_history` method is deprecated and may be removed in future versions. Use `account_tx` or `ledger` with `transactions: true` instead.
{% endhint %}

### Parameters

| Parameter | Type   | Required | Description    |
| --------- | ------ | -------- | -------------- |
| start     | number | No       | Starting index |

### Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "tx_history",
    "params": [{
        "start": 0
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'tx_history',
    params: [{
        start: 0
    }],
    id: 'getblock.io'
};

axios.post(url, payload, { headers })
    .then(response => console.log(response.data))
    .catch(error => console.error(error));
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "tx_history",
    "params": [{
        "start": 0
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
        "method": "tx_history",
        "params": [{
            "start": 0
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

### Response Example

```json
{
    "result": {
        "index": 0,
        "status": "success",
        "txs": []
    }
}
```

### Returns

| Field | Type   | Description       |
| ----- | ------ | ----------------- |
| index | number | Starting index    |
| txs   | array  | Transaction array |

### Recommended Alternatives

{% tabs %}
{% tab title="Use account_tx Instead" %}
```javascript
const xrpl = require('xrpl');

async function getTransactions() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'account_tx',
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        limit: 20
    });
    
    console.log('Transactions:', response.result.transactions);
    await client.disconnect();
}

getTransactions();
```
{% endtab %}

{% tab title="Use ledger with transactions" %}
```javascript
const xrpl = require('xrpl');

async function getLedgerTx() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'ledger',
        ledger_index: 'validated',
        transactions: true,
        expand: true
    });
    
    console.log('Transactions:', response.result.ledger.transactions);
    await client.disconnect();
}

getLedgerTx();
```
{% endtab %}
{% endtabs %}
