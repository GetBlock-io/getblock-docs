---
description: >-
  Example code for the price-estimate JSON RPC method. Сomplete guide on how to
  use price-estimate JSON RPC in GetBlock Web3 documentation.
---

# price-estimate - TRON Energy

This get a real-time price estimate for Energy delegation before placing an order.

### Body Parameter

| Parameter    | Type    | Required | Description                       |
| ------------ | ------- | -------- | --------------------------------- |
| resourceType | string  | Yes      | "energy"                          |
| volume       | integer | Yes      | Energy amount: 30,000 — 5,000,000 |
| duration     | string  | Yes      | "1h", "1d", "3d", "7d", "14d"     |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://api.getblock.io/tron-energy/delegateBandwidth \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "resourceType": "energy",
    "volume": 50000,
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
    "resourceType": "energy",
    "volume": 50000,
    "duration": "1h"
  }
);

const config = {
    method: 'post',
    url: 'https://api.getblock.io/tron-energy/price-estimate ',
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

url = "https://api.getblock.io/tron-energy/price-estimate "

payload = json.dumps({
    "resourceType": "energy",
    "volume": 50000,
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

    let url = "https://api.getblock.io/tron-energy/price-estimate ";
    let payload = r#"{
    "resourceType": "energy",
    "volume": 50000,
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
    url := "https://api.getblock.io/tron-energy/price-estimate "
    payload := map[string]interface{}{
    "resourceType": "energy",
    "volume": 50000,
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

### Response Sample

```bash
{
  "price_sun": 32,
  "trx": "2.08",
  "price_usd": "0.58",
  "provider": "default"
}
```
