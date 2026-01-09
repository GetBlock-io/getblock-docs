---
description: >-
  Example code for the suix_getDynamicFields JSON-RPC method. Complete guide on
  how to use suix_getDynamicFields JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getDynamicFields - Sui

This method returns the list of dynamic fields owned by an object on the SUI network. Dynamic fields allow objects to store key-value pairs that can be added or removed at runtime, enabling flexible data structures like maps and tables in Move.

## Parameters

| Parameter          | Type     | Required | Description            |
| ------------------ | -------- | -------- | ---------------------- |
| parent\_object\_id | ObjectID | Yes      | The parent object ID   |
| cursor             | ObjectID | No       | Pagination cursor      |
| limit              | uint     | No       | Maximum items per page |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_getDynamicFields",
  "params": [
    "0x5612581eba57ebe7e594b809ccceec2be4dac6ff6945d49b3ecc043d049611f6",
    null,
    10
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'suix_getDynamicFields',
  params: [
    '0x5612581eba57ebe7e594b809ccceec2be4dac6ff6945d49b3ecc043d049611f6',
    null,
    10
  ]
};

axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', payload)
.then(response => console.log(response.data));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "suix_getDynamicFields",
    "params": [
        "0x5612581eba57ebe7e594b809ccceec2be4dac6ff6945d49b3ecc043d049611f6",
        None,
        10
    ]
}

response = requests.post("https://go.getblock.io/<ACCESS-TOKEN>/", json=payload)
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
        "id": "getblock.io",
        "method": "suix_getDynamicFields",
        "params": ["0x5612581eba57ebe7e594b809ccceec2be4dac6ff6945d49b3ecc043d049611f6", null, 10]
    });
    let response = client.post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .json(&payload).send().await?;
    println!("{}", response.text().await?);
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
  "jsonrpc": "2.0",
  "result": {
    "data": [
      {
        "name": {
          "type": "0x1::string::String",
          "value": "key_name"
        },
        "bcsName": "a2V5X25hbWU=",
        "type": "DynamicField",
        "objectType": "0x2::dynamic_field::Field<0x1::string::String, u64>",
        "objectId": "0x1234...",
        "version": 100,
        "digest": "ABC123..."
      }
    ],
    "nextCursor": "0x1234...",
    "hasNextPage": true
  },
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter  | Type   | Description                         |
| ---------- | ------ | ----------------------------------- |
| name       | object | Dynamic field name (type and value) |
| type       | string | DynamicField or DynamicObject       |
| objectType | string | Full type of the field object       |
| objectId   | string | Object ID of the field              |
| version    | number | Object version                      |
| digest     | string | Object digest                       |

## Use Cases

* Enumerate map or table contents
* Browse object attachments
* Query dynamic storage
* Build data explorers

## Error Handling

| Error Code | Description                          |
| ---------- | ------------------------------------ |
| -32602     | Invalid params - malformed object ID |
| -32603     | Internal error - node issues         |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="sui.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const fields = await client.getDynamicFields({
  parentId: '0x5612581eba57ebe7e594b809ccceec2be4dac6ff6945d49b3ecc043d049611f6'
});
console.log(fields);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="sui_rust.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let parent = "0x5612581eba57ebe7e594b809ccceec2be4dac6ff6945d49b3ecc043d049611f6".parse()?;
    let fields = sui.read_api().get_dynamic_fields(parent, None, None).await?;
    println!("{:?}", fields);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
