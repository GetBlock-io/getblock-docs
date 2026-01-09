---
description: >-
  Example code for the suix_getBalance JSON-RPC method. Complete guide on how to
  use suix_getBalance JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getBalance - Sui

This method retrieves an account's total coin balance for a specified coin type on the SUI network, returning the aggregated balance across all coin objects owned by the address.

### Parameters

| Parameter  | Type   | Required | Description                                            | Default/Supported Values                       |
| ---------- | ------ | -------- | ------------------------------------------------------ | ---------------------------------------------- |
| owner      | string | Yes      | The owner's SUI address whose balance is being queried | Must be a valid SUI address starting with "0x" |
| coin\_type | string | No       | Type name for the coin to query                        | Defaults to "0x2::sui::SUI"                    |

### Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/ \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "suix_getBalance",
  "params": [
    "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961"
  ],
  "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {"Content-Type": "application/json"}

payload = {
    "jsonrpc": "2.0",
    "method": "suix_getBalance",
    "params": [
        "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961"
    ],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  jsonrpc: "2.0",
  method: "suix_getBalance",
  params: [
    "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961"
  ],
  id: "getblock.io"
};

axios.post(url, payload, {
  headers: { "Content-Type": "application/json" }
})
.then(response => {
  console.log("Result:", response.data.result);
})
.catch(error => {
  console.error("Error:", error.response?.status, error.response?.data);
});
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
        "id": "getblock.io",
        "method": "suix_getBalance",
        "params": [
            "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961"
        ]
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    println!("Response: {:?}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

### **Response**

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "coinType": "0x2::sui::SUI",
    "coinObjectCount": 15,
    "totalBalance": "3000000000",
    "lockedBalance": {}
  }
}
```

### Response Parameters

| Parameter              | Type    | Description                                      |
| ---------------------- | ------- | ------------------------------------------------ |
| jsonrpc                | string  | The JSON-RPC protocol version ("2.0")            |
| id                     | string  | Request identifier matching the request          |
| result                 | object  | The balance information object                   |
| result.coinType        | string  | The fully qualified coin type identifier         |
| result.coinObjectCount | integer | Number of coin objects of this type owned        |
| result.totalBalance    | string  | Total balance in MIST (1 SUI = 10^9 MIST)        |
| result.lockedBalance   | object  | Map of locked balance amounts by lock identifier |

### Use Cases

1.  Wallet Balance Display

    The primary use case for `suix_getBalance` is displaying the current balance of a SUI address in wallet applications. By calling this method, a wallet app can fetch the latest aggregated balance and display it in the user interface, allowing users to track their holdings in real-time.
2.  Transaction Validation

    Before initiating a transaction, developers can use `suix_getBalance` to ensure that a SUI address has sufficient funds to cover the transaction amount and associated gas fees. This pre-check helps prevent failed transactions due to insufficient balance.
3.  Gas Availability Check

    Applications can verify that an account has enough SUI for gas payments before constructing complex Programmable Transaction Blocks, improving user experience by failing fast when funds are insufficient.

### Error handling

| Error                  | Description                                            | Solution                                                                         |
| ---------------------- | ------------------------------------------------------ | -------------------------------------------------------------------------------- |
| Invalid Address Format | The SUI address is not properly formatted              | Ensure the address is a valid 66-character hexadecimal string starting with '0x' |
| Invalid Coin Type      | The specified coin type does not exist or is malformed | Verify the coin type follows the format "package::module::type"                  |
| Network Error          | Connection issues with the RPC endpoint                | Check network connectivity and endpoint availability                             |
| Rate Limiting          | Too many requests to the RPC endpoint                  | Implement request throttling or use dedicated node service                       |

### SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const balance = await client.getBalance({
  owner: '0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961',
});

console.log(balance);
```
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    let address = "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961".parse()?;
    let balance = sui.coin_read_api().get_balance(address, None).await?;
    
    println!("{:?}", balance);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
