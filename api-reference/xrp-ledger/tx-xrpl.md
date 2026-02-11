---
description: >-
  Example code for the tx JSON RPC method. Complete guide to using tx JSON-RPC
  in the GetBlock Web3 documentation.
---

# tx- XRPL

The `tx` method retrieves information on a single transaction, by its identifying hash.

## Parameters

| Parameter   | Type    | Required | Description                   |
| ----------- | ------- | -------- | ----------------------------- |
| transaction | string  | Yes      | 64-character transaction hash |
| binary      | boolean | No       | Return binary format          |
| min\_ledger | number  | No       | Minimum ledger to search      |
| max\_ledger | number  | No       | Maximum ledger to search      |

## Returns

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "tx",
    "params": [{
        "transaction": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9"
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
    method: 'tx',
    params: [{
        transaction: 'C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9'
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
    "method": "tx",
    "params": [{
        "transaction": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9"
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
        "method": "tx",
        "params": [{
            "transaction": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9"
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

```json
{
    "result": {
        "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "Amount": "1000000",
        "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
        "Fee": "12",
        "Sequence": 45,
        "TransactionType": "Payment",
        "hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
        "ledger_index": 63632029,
        "meta": {
            "TransactionResult": "tesSUCCESS"
        },
        "status": "success",
        "validated": true
    }
}
```

## Response Parameters

| Parameter         | Type   | Description         |
| ----------------- | ------ | ------------------- |
| TransactionType   | string | Type of transaction |
| TransactionResult | string | Outcome             |

| Field         | Type    | Description          |
| ------------- | ------- | -------------------- |
| hash          | string  | Transaction hash     |
| ledger\_index | integer | Ledger containing tx |
| meta          | object  | Transaction metadata |
| validated     | boolean | Whether validated    |

## Use Cases

* Look up transaction
* Verify payment status
* Check transaction details

### Error Handling

| Error Code    | Description           |
| ------------- | --------------------- |
| txnNotFound   | Transaction not found |
| invalidParams | Invalid parameters    |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function getTx() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'tx',
        transaction: 'C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9'
    });
    
    console.log('Status:', response.result.meta.TransactionResult);
    await client.disconnect();
}

getTx();
```
{% endtab %}

{% tab title="xrpl-py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import Tx

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = Tx(transaction="C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9")
response = client.request(request)
print(response.result)
```
{% endtab %}
{% endtabs %}
