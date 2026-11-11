---
description: Example code for the orders/{order_id} TRON Energy API endpoint
---

# orders/{order\_id} - TRON energy

This retrieves the details of a previously placed order. Use it to confirm a `202` Bandwidth order, or to look up the `txid` of any past delegation.

### Path Parameter

| Parameter  | Type   | Required | Description                                                            |
| ---------- | ------ | -------- | ---------------------------------------------------------------------- |
| order\_id  | string | Yes      | The `orderId` returned by `delegate-energy` or `delegate-bandwidth`    |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X GET https://services.getblock.io/v1/tron-energy/orders/0a6d59c4-9486-45b1-bf3f-74d075ad9b4c \
  -H "Authorization: Bearer YOUR_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const orderId = '0a6d59c4-9486-45b1-bf3f-74d075ad9b4c';
const response = await axios.get(`https://services.getblock.io/v1/tron-energy/orders/${orderId}`, {
  headers: { Authorization: 'Bearer YOUR_API_KEY' },
});
console.log(response.data);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests

order_id = "0a6d59c4-9486-45b1-bf3f-74d075ad9b4c"
url = f"https://services.getblock.io/v1/tron-energy/orders/{order_id}"
headers = {"Authorization": "Bearer YOUR_API_KEY"}

response = requests.get(url, headers=headers)
print(response.json())
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
    let order_id = "0a6d59c4-9486-45b1-bf3f-74d075ad9b4c";

    let response = client
        .get(format!("https://services.getblock.io/v1/tron-energy/orders/{}", order_id))
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
{% code overflow="wrap" %}
```go
package main

import (
    "fmt"
    "io"
    "net/http"
)

func main() {
    orderID := "0a6d59c4-9486-45b1-bf3f-74d075ad9b4c"
    req, _ := http.NewRequest("GET", fmt.Sprintf("https://services.getblock.io/v1/tron-energy/orders/%s", orderID), nil)
    req.Header.Set("Authorization", "Bearer YOUR_API_KEY")

    resp, err := http.DefaultClient.Do(req)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()

    body, _ := io.ReadAll(resp.Body)
    fmt.Println(string(body))
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Sample

```json
{
  "data": {
    "id": "0a6d59c4-9486-45b1-bf3f-74d075ad9b4c",
    "resource_type": "energy",
    "target_address": "TUo8pycbvje9w2XYsNnnzw67bpPs4GLFyD",
    "status": "charged",
    "volume": 65000,
    "duration": "1h",
    "txid": "63285f35942a0a8552fa7664b5fd8d4c2140640e9c766fe44fda7e01c97233df",
    "price_usd": "0.40",
    "created_at": "2026-11-03T10:52:13Z",
    "updated_at": "2026-11-03T10:52:13Z",
    "charged_at": "2026-11-03T10:52:13Z"
  }
}
```

### Response Sample Definition

| Field           | Type    | Description                                                         |
| --------------- | ------- | ------------------------------------------------------------------- |
| id              | string  | Order ID                                                            |
| resource\_type  | string  | `"energy"` or `"bandwidth"`                                         |
| target\_address | string  | Address that received the resources                                 |
| status          | string  | Order status (see below)                                            |
| volume          | integer | Amount delegated                                                    |
| duration        | string  | Rental period                                                       |
| txid            | string  | Delegation transaction hash. Empty until delivered                  |
| price\_usd      | string  | Amount charged. Empty until charged                                 |
| created\_at     | string  | RFC 3339 time the order was placed                                  |
| updated\_at     | string  | RFC 3339 time of the last status change                             |
| charged\_at     | string  | RFC 3339 time your balance was charged                              |

### Order Status

| Status     | Meaning                                                              |
| ---------- | -------------------------------------------------------------------- |
| `accepted` | Order accepted by the provider, awaiting on-chain confirmation       |
| `charged`  | Resources delegated on-chain and your balance charged                |

An unknown `order_id` returns `404` with `{"data": null, "error": "order not found"}`.
