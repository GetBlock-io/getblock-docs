---
description: >-
  Example code for the suix_queryEvents JSON-RPC method. Complete guide on how
  to use suix_queryEvents JSON-RPC in GetBlock Web3 documentation.
---

# suix\_queryEvents - Sui

This method returns events matching specified query criteria on the SUI network. Events are emitted by Move modules during transaction execution and provide powerful tracking of on-chain activities.

### Parameters

| Parameter         | Type    | Required | Description            | Default/Supported Values |
| ----------------- | ------- | -------- | ---------------------- | ------------------------ |
| query             | object  | Yes      | Event filter criteria  | EventFilter object       |
| cursor            | object  | No       | Paging cursor          | EventID object           |
| limit             | integer | No       | Maximum items per page | Default varies           |
| descending\_order | boolean | No       | Sort order             | false (ascending)        |

### Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "suix_queryEvents",
  "params": [
    {"MoveModule": {"package": "0xa395759ca37c6e1ffc179184e98a6f9a2da5d78f6e34b0e5044ed52a6bc0a1bc", "module": "test"}},
    null,
    100,
    false
  ],
  "id": "getblock.io"
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
  "method": "suix_queryEvents",
  "params": [
    {"MoveModule": {"package": "0xa395759ca37c6e1ffc179184e98a6f9a2da5d78f6e34b0e5044ed52a6bc0a1bc", "module": "test"}},
    null,
    100,
    false
  ],
  "id": "getblock.io"
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
  "method": "suix_queryEvents",
  "params": [
    {"MoveModule": {"package": "0xa395759ca37c6e1ffc179184e98a6f9a2da5d78f6e34b0e5044ed52a6bc0a1bc", "module": "test"}},
    null,
    100,
    false
  ],
  "id": "getblock.io"
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
            "method": "suix_queryEvents",
            "params": [
              {"MoveModule": {"package": "0xa395759ca37c6e1ffc179184e98a6f9a2da5d78f6e34b0e5044ed52a6bc0a1bc", "module": "test"}},
              null,
              100,
              false
            ],
            "id": "getblock.io"
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

### Request Sample

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "data": [
      {
        "id": {"txDigest": "FFwCMgC7FHBLEwfL9JeSeR2EhMAZMykUPVW1kE3HgTMe", "eventSeq": "1"},
        "packageId": "0xb2fd632992b01aa25900867288b63d6255ff8223c12b0fd985c49d5777a0d65a",
        "transactionModule": "test",
        "sender": "0xcceee09f44d558691334ec0aff47af033f57162a2f33056e2585e2c46863ac02",
        "type": "0x3::test::TestEvent",
        "parsedJson": {"value": "example"}
      }
    ],
    "nextCursor": {"txDigest": "FFwCMgC7FHBLEwfL9JeSeR2EhMAZMykUPVW1kE3HgTMe", "eventSeq": "1"},
    "hasNextPage": false
  }
}
```

### Body Parameters

| Parameter                 | Type    | Description                |
| ------------------------- | ------- | -------------------------- |
| result.data               | array   | Array of matching events   |
| result.data\[].id         | object  | Event identifier           |
| result.data\[].packageId  | string  | Package that emitted event |
| result.data\[].type       | string  | Event type                 |
| result.data\[].parsedJson | object  | Event data as JSON         |
| result.nextCursor         | object  | Cursor for pagination      |
| result.hasNextPage        | boolean | More results available     |

### Use Cases

* Event Monitoring: Track smart contract events in real-time for notifications.
* Analytics: Build event-driven analytics and reporting dashboards.
* Audit Trails: Create audit logs of contract interactions.

### Errors Handling

| Error          |                         |                                |
| -------------- | ----------------------- | ------------------------------ |
| Invalid Filter | Malformed event filter  | Check filter syntax            |
| Invalid Cursor | Malformed cursor object | Use valid cursor from response |

### SDK Integration

{% code title="Sui TypeScript SDK" %}
```typescript
import { SuiClient } from '@mysten/sui/client';
const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const events = await client.queryEvents({
  query: { MoveModule: { package: '0x2', module: 'coin' } },
  limit: 100
});
console.log(events);
```
{% endcode %}
