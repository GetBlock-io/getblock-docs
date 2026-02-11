---
description: >-
  Example code for the book_offers JSON RPC method. Сomplete guide on how to use
  book_offers JSON RPC in GetBlock Web3 documentation.
---

# book\_offers - XRPL

This method retrieves a list of offers, also known as the order book, between two currencies.

## Parameters

| Parameter     | Type          | Required | Description             |
| ------------- | ------------- | -------- | ----------------------- |
| taker\_gets   | object        | Yes      | Currency taker receives |
| taker\_pays   | object        | Yes      | Currency taker pays     |
| ledger\_hash  | string        | No       | Ledger hash             |
| ledger\_index | string/number | No       | Ledger index            |
| limit         | number        | No       | Maximum results         |
| taker         | string        | No       | Perspective account     |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "book_offers",
    "params": [{
        "taker_gets": {"currency": "XRP"},
        "taker_pays": {
            "currency": "USD",
            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"
        },
        "limit": 10
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="book_offers.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'book_offers',
    params: [{
        taker_gets: { currency: 'XRP' },
        taker_pays: {
            currency: 'USD',
            issuer: 'rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B'
        },
        limit: 10
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
{% code title="book_offers.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "book_offers",
    "params": [{
        "taker_gets": {"currency": "XRP"},
        "taker_pays": {
            "currency": "USD",
            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"
        },
        "limit": 10
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="book_offers.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "book_offers",
        "params": [{
            "taker_gets": {"currency": "XRP"},
            "taker_pays": {
                "currency": "USD",
                "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"
            },
            "limit": 10
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
        "offers": [
            {
                "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
                "BookDirectory": "...",
                "BookNode": "0",
                "Flags": 0,
                "OwnerNode": "0",
                "Sequence": 100,
                "TakerGets": "1000000",
                "TakerPays": {
                    "currency": "USD",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "1"
                }
            }
        ],
        "status": "success"
    }
}
```

## Response Parameters

| Parameter              | Type          | Description        |
| ---------------------- | ------------- | ------------------ |
| offers                 | array         | Order book entries |
| TakerGets              | object/string | Receiving amount   |
| TakerPays              | object/string | Paying amount      |
| ledger\_current\_index | integer       | current ledger     |

## Use Cases

* View order book
* Build trading UI
* Calculate exchange rates

## Error Handling

| Error Code    | Description        |
| ------------- | ------------------ |
| invalidParams | Invalid parameters |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function getOffers() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'book_offers',
        taker_gets: { currency: 'XRP' },
        taker_pays: {
            currency: 'USD',
            issuer: 'rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B'
        }
    });
    
    console.log('Offers:', response.result.offers.length);
    await client.disconnect();
}

getOffers();
```
{% endtab %}

{% tab title="xrpl-py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import BookOffers
from xrpl.models.currencies import XRP, IssuedCurrency

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = BookOffers(
    taker_gets=XRP(),
    taker_pays=IssuedCurrency(currency="USD", issuer="rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B")
)
response = client.request(request)
print(response.result)
```
{% endtab %}
{% endtabs %}
