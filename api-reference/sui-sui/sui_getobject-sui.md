---
description: >-
  Example code for the sui_getObject JSON-RPC method. Complete guide on how to
  use sui_getObject JSON-RPC in GetBlock Web3 documentation.
---

# sui\_getObject - Sui

This method retrieves comprehensive information about a specified object on the SUI network, including its type, owner, content, version history, and storage details based on configurable options.

### Parameters

| Parameter  | Type   | Required | Description                                  | Default/Supported Values                    |
| ---------- | ------ | -------- | -------------------------------------------- | ------------------------------------------- |
| object\_id | string | Yes      | The unique identifier of the object to query | Must be a valid SUI object ID (0x prefixed) |
| options    | object | No       | Options specifying which data to include     | See ObjectDataOptions below                 |

ObjectDataOptions

| Field                   | Type    | Default | Description                                          |
| ----------------------- | ------- | ------- | ---------------------------------------------------- |
| showType                | boolean | false   | Include the object's type information                |
| showOwner               | boolean | false   | Include owner information                            |
| showPreviousTransaction | boolean | false   | Include the digest of the last modifying transaction |
| showDisplay             | boolean | false   | Include display fields for NFTs                      |
| showContent             | boolean | false   | Include the object's Move content                    |
| showBcs                 | boolean | false   | Include BCS-encoded content                          |
| showStorageRebate       | boolean | false   | Include storage rebate information                   |

### Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/ \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "sui_getObject",
  "params": [
    "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
    {
      "showType": true,
      "showOwner": true,
      "showPreviousTransaction": true,
      "showContent": true,
      "showStorageRebate": true
    }
  ],
  "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {"Content-Type": "application/json"}

payload = {
    "jsonrpc": "2.0",
    "method": "sui_getObject",
    "params": [
        "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
        {
            "showType": True,
            "showOwner": True,
            "showContent": True
        }
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

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  jsonrpc: "2.0",
  method: "sui_getObject",
  params: [
    "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
    {
      showType: true,
      showOwner: true,
      showContent: true
    }
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
{% code title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "sui_getObject",
        "params": [
            "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
            {
                "showType": true,
                "showOwner": true,
                "showContent": true
            }
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

### Response

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "data": {
      "objectId": "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
      "version": "1",
      "digest": "33K5ZXJ3RyubvYaHuEnQ1QXmmbhgtrFwp199dnEbL4n7",
      "type": "0x2::coin::Coin<0x2::sui::SUI>",
      "owner": {
        "AddressOwner": "0xc8ec1d5b84dd6289e193b9f88de4a994358c9f856135236c3e75a925e1c77ac3"
      },
      "previousTransaction": "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
      "storageRebate": "100",
      "content": {
        "dataType": "moveObject",
        "type": "0x2::coin::Coin<0x2::sui::SUI>",
        "hasPublicTransfer": true,
        "fields": {
          "balance": "100000000",
          "id": {
            "id": "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809"
          }
        }
      }
    }
  }
}
```

### Response Parameters

| Parameter                       | Type   | Description                                                         |
| ------------------------------- | ------ | ------------------------------------------------------------------- |
| jsonrpc                         | string | The JSON-RPC protocol version ("2.0")                               |
| id                              | string | Request identifier                                                  |
| result.data                     | object | The object data if found                                            |
| result.data.objectId            | string | Unique object identifier                                            |
| result.data.version             | string | Object version number                                               |
| result.data.digest              | string | Object digest hash                                                  |
| result.data.type                | string | Fully qualified Move type                                           |
| result.data.owner               | object | Owner information (AddressOwner, ObjectOwner, Shared, or Immutable) |
| result.data.previousTransaction | string | Digest of last modifying transaction                                |
| result.data.storageRebate       | string | Storage rebate in MIST                                              |
| result.data.content             | object | Object content including Move fields                                |

### Use Cases

1. **NFT Metadata Retrieval:** Fetch NFT metadata, display fields, and ownership information for marketplace applications or NFT galleries by querying the object with `showDisplay` and `showContent` options.
2. **Object Ownership Verification:** Verify object ownership before initiating transactions by checking the owner field, essential for security validations in dApps.
3. **Smart Contract State Inspection:** Inspect the state of smart contract instances by retrieving their Move content, enabling debugging and state verification.

### Error handling

| Code              | Description                             | Solution                                                 |
| ----------------- | --------------------------------------- | -------------------------------------------------------- |
| Invalid Object ID | The object ID is not properly formatted | Ensure the ID is a valid 66-character hexadecimal string |
| NotExists         | Object does not exist on-chain          | Verify the object ID is correct and exists               |
| Deleted           | Object has been deleted                 | The object was consumed or deleted in a transaction      |
| UnknownVersion    | Requested version not found             | Use `sui_tryGetPastObject` for historical versions       |



### SDK Integration

{% tabs %}
{% tab title="Sui Typescript SDK" %}
{% code title="SUI TypeScript SDK" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const object = await client.getObject({
  id: '0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809',
  options: {
    showType: true,
    showOwner: true,
    showContent: true,
  },
});

console.log(object);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDk" %}
{% code title="SUI Rust SDK" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;
use sui_sdk::types::base_types::ObjectID;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    let object_id: ObjectID = "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809".parse()?;
    let object = sui.read_api().get_object_with_options(object_id, None).await?;
    
    println!("{:?}", object);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
