---
description: >-
  Example code for the sui_multiGetObjects JSON-RPC method. Complete guide on
  how to use sui_multiGetObjects JSON-RPC in GetBlock Web3 documentation.
---

# sui\_multiGetObjects - Sui

This method returns object data for a list of objects on the SUI network in a single request. This batch method is more efficient than making individual `sui_getObject` calls when you need data for multiple objects.

## Parameters

| Parameter   | Type              | Required | Description                  |
| ----------- | ----------------- | -------- | ---------------------------- |
| object\_ids | array             | Yes      | Array of object IDs to query |
| options     | ObjectDataOptions | No       | Options for response content |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_multiGetObjects",
  "params": [
    [
      "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
      "0x11af4b844ff94b3fbef6e36b518da3ad4c5856fa686464524a876b463d129760"
    ],
    {
      "showType": true,
      "showOwner": true,
      "showContent": true
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
  "method": "sui_multiGetObjects",
  "params": [
    [
      "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
      "0x11af4b844ff94b3fbef6e36b518da3ad4c5856fa686464524a876b463d129760"
    ],
    {
      "showType": true,
      "showOwner": true,
      "showContent": true
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
  "method": "sui_multiGetObjects",
  "params": [
    [
      "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
      "0x11af4b844ff94b3fbef6e36b518da3ad4c5856fa686464524a876b463d129760"
    ],
    {
      "showType": true,
      "showOwner": true,
      "showContent": true
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
            "method": "sui_multiGetObjects",
            "params": [
              [
                "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
                "0x11af4b844ff94b3fbef6e36b518da3ad4c5856fa686464524a876b463d129760"
              ],
              {
                "showType": true,
                "showOwner": true,
                "showContent": true
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

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "data": {
        "objectId": "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
        "version": "1",
        "digest": "33K5ZXJ3RyubvYaHuEnQ1QXmmbhgtrFwp199dnEbL4n7",
        "type": "0x2::coin::Coin<0x2::sui::SUI>"
      }
    }
  ],
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type  | Description               |
| --------- | ----- | ------------------------- |
| result    | array | Array of object responses |

## Use Cases

* Fetch multiple NFTs efficiently
* Load coin objects for transaction building
* Batch validate object existence
* Optimize API calls for large datasets

## Error Handling

| Error Code | Description                           |
| ---------- | ------------------------------------- |
| -32602     | Invalid params - malformed object IDs |
| -32603     | Internal error - node issues          |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="example.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const objects = await client.multiGetObjects({
  ids: ['0x53e4567...', '0x11af4b8...'],
  options: { showType: true, showContent: true }
});
console.log(objects);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="example_rust.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let ids = vec!["0x53e4567...".parse()?, "0x11af4b8...".parse()?];
    let objects = sui.read_api().multi_get_object_with_options(ids, None).await?;
    println!("{:?}", objects);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
