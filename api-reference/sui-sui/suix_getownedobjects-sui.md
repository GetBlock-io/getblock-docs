---
description: >-
  Example code for the suix_getOwnedObjects JSON-RPC method. Complete guide on
  how to use suix_getOwnedObjects JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getOwnedObjects - Sui

This method returns a list of objects owned by an address on the SUI network with powerful filtering capabilities. This method supports pagination and allows filtering by object type, module, package, or other criteria.

### Parameters

| Parameter | Type    | Required | Description                  | Default/Supported Values   |
| --------- | ------- | -------- | ---------------------------- | -------------------------- |
| address   | string  | Yes      | The owner's SUI address      | Valid SUI address          |
| query     | object  | No       | Query criteria for filtering | ObjectResponseQuery object |
| cursor    | string  | No       | Pagination cursor            | Object ID for cursor       |
| limit     | integer | No       | Maximum items per page       | Default varies by node     |

### Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/ \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "suix_getOwnedObjects",
  "params": [
    "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961",
    {
      "filter": {"StructType": "0x2::coin::Coin<0x2::sui::SUI>"},
      "options": {"showType": true, "showOwner": true}
    },
    null,
    10
  ],
  "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
payload = {
    "jsonrpc": "2.0",
    "method": "suix_getOwnedObjects",
    "params": ["0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961", None, None, 10],
    "id": "getblock.io"
}
response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print("Result:", response.json().get("result"))
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
axios.post("https://go.getblock.io/<ACCESS-TOKEN>/", {
  jsonrpc: "2.0",
  method: "suix_getOwnedObjects",
  params: ["0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961", null, null, 10],
  id: "getblock.io"
}).then(r => console.log("Result:", r.data.result));
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
        "jsonrpc": "2.0", "id": "getblock.io",
        "method": "suix_getOwnedObjects",
        "params": ["0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961", null, null, 10]
    });
    let response = client.post("https://go.getblock.io/<ACCESS-TOKEN>/").json(&payload).send().await?;
    println!("Response: {:?}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **Response**

{% code overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "data": [
      {
        "data": {
          "objectId": "0x0b37a91692359a98496738a58c17a9334aeacc435c70ab9635e47a277d8f8dd9",
          "version": "13488",
          "type": "0x2::coin::Coin<0x2::sui::SUI>",
          "owner": {"AddressOwner": "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961"}
        }
      }
    ],
    "nextCursor": "0xe26860fac6839ce2d7ed7e6f29d276a1b4c23f2d9a9b6f0d8b2c17beace292b7",
    "hasNextPage": true
  }
}
```
{% endcode %}

### Response Parameters

| Parameter          | Type    | Description              |
| ------------------ | ------- | ------------------------ |
| result.data        | array   | Array of owned objects   |
| result.nextCursor  | string  | Cursor for next page     |
| result.hasNextPage | boolean | Whether more pages exist |

### Use Cases

1.  NFT Gallery

    List all NFTs owned by an address for gallery displays.
2.  Wallet Inventory

    Display all objects and assets owned by a wallet.
3.  Filtered Queries

    Find specific token types using struct filters.

### Error Handling

| Error           | Description             | Solution              |
| --------------- | ----------------------- | --------------------- |
| Invalid Address | Malformed SUI address   | Verify address format |
| Invalid Filter  | Malformed filter object | Check filter syntax   |

### SDK Integration

{% code title="Sui TypeScript SDK" %}
```typescript
import { SuiClient } from '@mysten/sui/client';
const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const objects = await client.getOwnedObjects({
  owner: '0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961',
  options: { showType: true }
});
console.log(objects);
```
{% endcode %}
