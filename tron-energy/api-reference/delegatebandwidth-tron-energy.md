---
description: >-
  Example code for the delegateBandwidth JSON RPC method. Сomplete guide on how
  to use delegateBandwidth JSON RPC in GetBlock Web3 documentation.
hidden: true
---

# delegateBandwidth - TRON energy

This delegates bandwidth to a TRON address. Same flow as Energy — instant result in most cases.

### Body Parameter

| Parameter       | Type    | Required | Description                                        |
| --------------- | ------- | -------- | -------------------------------------------------- |
| target\_address | string  | Yes      | TRON wallet address (starts with T, 34 characters) |
| volume          | integer | Yes      | Bandwidth amount: 1,000 — 200,000                  |
| duration        | string  | Yes      | "1h" (only 1 hour supported for Bandwidth)         |

### Request Sample

#### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://api.getblock.io/tron-energy/delegateBandwidth \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 5000,
    "duration": "1h"
  }
'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';
const data = JSON.stringify({
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 5000,
    "duration": "1h"
  }
);

const config = {
    method: 'post',
    url: 'https://api.getblock.io/tron-energy/delegateBandwidth',
    headers: {
        'Content-Type': 'application/json',
        'Authorization: Bearer YOUR_API_KEY'
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

url = "https://api.getblock.io/tron-energy/delegateBandwidth"

payload = json.dumps({
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 5000,
    "duration": "1h"
  }
)

headers = {
        'Content-Type': 'application/json',
        'Authorization: Bearer YOUR_API_KEY'
    },
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

    let url = "https://api.getblock.io/tron-energy/delegateBandwidth";
    let payload = r#"{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 5000,
    "duration": "1h"
  }
"#;

    let response = client
        .post(url)
        .header(header::CONTENT_TYPE, "application/json")
        .header(header::AUTHORIZATION, "Bearer YOUR_API_KEY")
        .body(payload)
        .send()
        .await?;

    println!("{}", response.text().await?);

    Ok(())
}
```
{% endcode %}
{% endtab %}

{% tab title="GO" %}
```go
package main
import (
    "bytes"
    "encoding/json"
    "fmt"
    "net/http"
)
func main() {
    url := "https://api.getblock.io/tron-energy/delegateBandwidth"
    payload := map[string]interface{}{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 5000,
    "duration": "1h"
  }

    jsonData, _ := json.Marshal(payload)
    req, _ := http.NewRequest("POST", url, bytes.NewBuffer(jsonData))
    req.Header.Set("Authorization", "Bearer YOUR_API_KEY")
    req.Header.Set("Content-Type", "application/json")
    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()
    
    var result map[string]interface{}
    json.NewDecoder(resp.Body).Decode(&result)
    fmt.Println(result)
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Bandwidth only supports "1h" duration.
{% endhint %}

### Response Sample

```bash
{
  "order_id": "clxyz123...",
  "status": "success",
  "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
  "resource_type": "energy",
  "volume": 65000,
  "duration": "3d",
  "price_usd": "4.97",
  "trx_spent": 20.12,
  "provider_order_id": 12345,
  "txid": "abc123def...",
  "message": "Order completed successfully."
}
```
