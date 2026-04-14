---
description: >-
  Example code for the orders/{order_id} JSON RPC method. Сomplete guide on how
  to use orders/{order_id} JSON RPC in GetBlock Web3 documentation.
---

# orders/{order\_id} - TRON energy

This retrieves the details of a previously placed order. Useful if you received a 202 response and need to check the final status.

### Path Parameter

| pending | Order is being processed                          |
| ------- | ------------------------------------------------- |
| success | Order completed successfully, resources delegated |
| failed  | Order failed — no charge applied                  |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X GET "https://api.getblock.io/tron-energy/orders/success" \
  -H "Authorization: Bearer YOUR_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
const axios = require('axios');

const orderId = "success";
const url = `https://api.getblock.io/tron-energy/orders/${orderId}`;

axios.get(url, {
    headers: {
        'Authorization': 'Bearer YOUR_API_KEY'
    }
})
.then(response => {
    console.log(response.data);
})
.catch(error => {
    console.error(error.response.data);
});
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests

order_id = "success"
url = f"https://api.getblock.io/tron-energy/orders/{order_id}"

headers = {
    'Authorization': 'Bearer YOUR_API_KEY'
}

response = requests.get(url, headers=headers)
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

    let order_id = "success";
    let url = format!("https://api.getblock.io/tron-energy/orders/{}", order_id);

    let response = client
        .get(&url)
        .header(header::AUTHORIZATION, "Bearer YOUR_API_KEY")
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
    "fmt"
    "io"
    "net/http"
)

func main() {
    orderId := "success"
    url := fmt.Sprintf("https://api.getblock.io/tron-energy/orders/%s", orderId)

    req, _ := http.NewRequest("GET", url, nil)
    req.Header.Set("Authorization", "Bearer YOUR_API_KEY")

    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()

    body, _ := io.ReadAll(resp.Body)
    fmt.Println(string(body))
}
```
{% endtab %}
{% endtabs %}

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
  "provider_order_id": "12345",
  "created_at": "2026-02-08T12:00:00.000Z"
}

```
