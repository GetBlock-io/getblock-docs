---
description: >-
  Example code for the suix_getAllBalances JSON-RPC method. Complete guide on
  how to use suix_getAllBalances JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getAllBalances - Sui

This method retrieves all coin balances for every token type owned by an address on the SUI network, returning aggregated balance information for each coin type in a single response.

### Parameters

| Parameter | Type   | Required | Description                                       | Default/Supported Values                       |
| --------- | ------ | -------- | ------------------------------------------------- | ---------------------------------------------- |
| owner     | string | Yes      | The owner's SUI address to query all balances for | Must be a valid SUI address starting with "0x" |

### Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/ \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "suix_getAllBalances",
  "params": [
    "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961"
  ],
  "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Python" %}
{% code title="suix_getAllBalances.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {"Content-Type": "application/json"}

payload = {
    "jsonrpc": "2.0",
    "method": "suix_getAllBalances",
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
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="suix_getAllBalances.js" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  jsonrpc: "2.0",
  method: "suix_getAllBalances",
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
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="suix_getAllBalances.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "suix_getAllBalances",
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
{% endcode %}
{% endtab %}
{% endtabs %}

### Sample Response

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": [
    {
      "coinType": "0x2::sui::SUI",
      "coinObjectCount": 15,
      "totalBalance": "3000000000",
      "lockedBalance": {}
    },
    {
      "coinType": "0x168da5bf1f48dafc111b0a488fa454aca95e0b5e::usdc::USDC",
      "coinObjectCount": 3,
      "totalBalance": "1500000000",
      "lockedBalance": {}
    }
  ]
}
```

### Response Parameters

| Parameter                 | Type    | Description                                 |
| ------------------------- | ------- | ------------------------------------------- |
| jsonrpc                   | string  | The JSON-RPC protocol version ("2.0")       |
| id                        | string  | Request identifier matching the request     |
| result                    | array   | Array of balance objects for each coin type |
| result\[].coinType        | string  | The fully qualified coin type identifier    |
| result\[].coinObjectCount | integer | Number of coin objects for this type        |
| result\[].totalBalance    | string  | Total balance in the coin's smallest unit   |
| result\[].lockedBalance   | object  | Map of locked balance amounts               |

### Use Cases

1. **Portfolio Display:** Display complete token holdings in wallet applications by fetching all balances in a single call, enabling comprehensive portfolio views without multiple API requests.
2. **Multi-Token Analytics:** Build analytics dashboards that track holdings across all token types, calculating total portfolio value by aggregating balances and market prices.
3. **Account Overview:** Provide users with a complete overview of their SUI account holdings, including native SUI and all custom tokens they own.

### Error handling

| Code                   | Description                               | Solution                                                                         |
| ---------------------- | ----------------------------------------- | -------------------------------------------------------------------------------- |
| Invalid Address Format | The SUI address is not properly formatted | Ensure the address is a valid 66-character hexadecimal string starting with '0x' |
| Empty Result           | Address has no coin objects               | Verify the address has received tokens                                           |
| Network Error          | Connection issues with the RPC endpoint   | Check network connectivity and endpoint availability                             |

### SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="TypeScript" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const balances = await client.getAllBalances({
  owner: '0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961',
});

console.log(balances);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="Rust (SDK)" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    let address = "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961".parse()?;
    let balances = sui.coin_read_api().get_all_balances(address).await?;
    
    println!("{:?}", balances);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
