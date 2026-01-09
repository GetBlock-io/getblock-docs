---
description: >-
  Example code for the suix_getDynamicFieldObject JSON-RPC method. Complete
  guide on how to use suix_getDynamicFieldObject JSON-RPC in GetBlock Web3
  documentation.
---

# suix\_getDynamicFieldObject - Sui

This method returns the object information of a specific dynamic field on the SUI network. While `suix_getDynamicFields` lists all dynamic fields. This method retrieves the full data for a single dynamic field when you know its name and type.

## Parameters

| Parameter          | Type             | Required | Description                             |
| ------------------ | ---------------- | -------- | --------------------------------------- |
| parent\_object\_id | ObjectID         | Yes      | The parent object ID                    |
| name               | DynamicFieldName | Yes      | The dynamic field name (type and value) |

### Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_getDynamicFieldObject",
  "params": [
    "0x3ddea0f8c3da994d9ead562ce76e36fdef6a382da344930c73d1298b0e9644b8",
    {
      "type": "0x1::string::String",
      "value": "key_name"
    }
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_getDynamicFieldObject",
  "params": [
    "0x3ddea0f8c3da994d9ead562ce76e36fdef6a382da344930c73d1298b0e9644b8",
    {
      "type": "0x1::string::String",
      "value": "key_name"
    }
  ]
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_getDynamicFieldObject",
  "params": [
    "0x3ddea0f8c3da994d9ead562ce76e36fdef6a382da344930c73d1298b0e9644b8",
    {
      "type": "0x1::string::String",
      "value": "key_name"
    }
  ]
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "suix_getDynamicFieldObject",
            "params": [
              "0x3ddea0f8c3da994d9ead562ce76e36fdef6a382da344930c73d1298b0e9644b8",
              {
                "type": "0x1::string::String",
                "value": "key_name"
              }
            ]
          }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "result": {
    "data": {
      "objectId": "0x1234...",
      "version": "100",
      "digest": "ABC123...",
      "type": "0x2::dynamic_field::Field<0x1::string::String, u64>",
      "content": {
        "dataType": "moveObject",
        "fields": {
          "id": { "id": "0x1234..." },
          "name": "key_name",
          "value": "42"
        }
      }
    }
  },
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter | Type   | Description                 |
| --------- | ------ | --------------------------- |
| objectId  | string | The dynamic field object ID |
| version   | string | Object version              |
| digest    | string | Object digest               |
| type      | string | Full Move type              |
| content   | object | Object content with fields  |

## Use Cases

* Retrieve specific map entries
* Read table values by key
* Access object attachments
* Query nested dynamic data

## Error Handling

| Error Code | Description                      |
| ---------- | -------------------------------- |
| -32602     | Invalid params - malformed input |
| -32603     | Internal error - node issues     |
| NotFound   | Dynamic field not found          |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const field = await client.getDynamicFieldObject({
  parentId: '0x3ddea0f8c3da994d9ead562ce76e36fdef6a382da344930c73d1298b0e9644b8',
  name: { type: '0x1::string::String', value: 'key_name' }
});
console.log(field);
```
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let parent = "0x3ddea0f8c3da994d9ead562ce76e36fdef6a382da344930c73d1298b0e9644b8".parse()?;
    // Use get_dynamic_field_object
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
