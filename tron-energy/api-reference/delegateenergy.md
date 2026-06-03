---
description: >-
  Example code for the delegateEnergy JSON RPC method. Сomplete guide on how to
  use delegateEnergy JSON RPC in GetBlock Web3 documentation.
---

# delegateEnergy - TRON energy

This endpoint delegates energy to a TRON address. In most cases, the order completes immediately with status "success".

#### Parameter

| Parameter       | Type    | Required | Description                                        |
| --------------- | ------- | -------- | -------------------------------------------------- |
| target\_address | string  | Yes      | TRON wallet address (starts with T, 34 characters) |
| volume          | integer | Yes      | Energy amount: 30,000 — 5,000,000                  |
| duration        | string  | Yes      | "1h", "1d", "3d", "7d", "14d"                      |

#### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://api.getblock.io/tron-energy/delegateEnergy \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 65000,
    "duration": "3d"
  }'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';
const data = JSON.stringify({
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 65000,
    "duration": "3d"
  });

const config = {
    method: 'post',
    url: 'https://api.getblock.io/tron-energy/delegateEnergy',
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

url = "https://api.getblock.io/tron-energy/delegateEnergy"

payload = json.dumps({
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "volume": 65000,
    "duration": "3d"
  })

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

    let url = "https://api.getblock.io/tron-energy/delegateEnergy";
    let payload = r#"{
        "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
        "volume": 65000,
        "duration": "1d"
    }"#;

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
    url := "https://api.getblock.io/tron-energy/delegateEnergy"
    payload := map[string]interface{}{
        "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
        "volume":         65000,
        "duration":       "3d",
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

#### &#x20;Response&#x20;

```json
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
