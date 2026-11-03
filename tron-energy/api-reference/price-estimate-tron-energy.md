---
description: Example code for the price-estimate TRON Energy API endpoint
---

# price-estimate - TRON energy

This returns a real-time price quote for an Energy or Bandwidth delegation before you place an order. It does not charge your balance.

### Body Parameter

| Parameter    | Type    | Required | Description                                                                 |
| ------------ | ------- | -------- | --------------------------------------------------------------------------- |
| resourceType | string  | Yes      | `"energy"` or `"bandwidth"`                                                 |
| volume       | integer | Yes      | Energy: 32,000 — 5,000,000. Bandwidth: 1,000 — 50,000                       |
| duration     | string  | Yes      | Energy: `"15m"`, `"1h"`, `"1d"`, `"3d"`, `"7d"`, `"14d"`. Bandwidth: `"1h"`, `"3d"`, `"7d"`, `"14d"` |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://services.getblock.io/v1/tron-energy/price-estimate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "resourceType": "energy",
  "volume": 65000,
  "duration": "1h"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const response = await axios.post(
  'https://services.getblock.io/v1/tron-energy/price-estimate',
  {
    "resourceType": "energy",
    "volume": 65000,
    "duration": "1h"
  },
  {
    headers: {
      'Content-Type': 'application/json',
      Authorization: 'Bearer YOUR_API_KEY',
    },
  }
);
console.log(response.data);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests

url = "https://services.getblock.io/v1/tron-energy/price-estimate"
payload = {
  "resourceType": "energy",
  "volume": 65000,
  "duration": "1h"
}
headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_API_KEY',
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
use reqwest::header;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();

    let response = client
        .post("https://services.getblock.io/v1/tron-energy/price-estimate")
        .header(header::AUTHORIZATION, "Bearer YOUR_API_KEY")
        .json(&json!({
          "resourceType": "energy",
          "volume": 65000,
          "duration": "1h"
        }))
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
    "bytes"
    "encoding/json"
    "fmt"
    "io"
    "net/http"
)

func main() {
    payload, _ := json.Marshal(map[string]interface{}{
        "resourceType": "energy",
        "volume": 65000,
        "duration": "1h",
    })

    req, _ := http.NewRequest("POST", "https://services.getblock.io/v1/tron-energy/price-estimate", bytes.NewBuffer(payload))
    req.Header.Set("Authorization", "Bearer YOUR_API_KEY")
    req.Header.Set("Content-Type", "application/json")

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
    "price_sun": "19",
    "trx": "1.2350",
    "price_usd": "0.4000",
    "reserve_usd": "0.6400"
  }
}
```

### Response Sample Definition

| Field        | Type   | Description                                                                                  |
| ------------ | ------ | -------------------------------------------------------------------------------------------- |
| price\_sun   | string | Price per unit of Energy (or Bandwidth) in SUN for the whole rental period. 1 TRX = 1,000,000 SUN |
| trx          | string | Total price in TRX (`price_sun × volume ÷ 1,000,000`)                                        |
| price\_usd   | string | Quoted total in USD. The final charge is the order's `price_usd` and can differ             |
| reserve\_usd | string | Reserve amount in USD returned with the quote                                                |

{% hint style="warning" %}
`price_sun` covers the **whole rental period**, so it rises steeply with `duration`. For 65,000 Energy, a `1h` rental costs about 1.2 TRX while a `1d` rental costs about 8.6 TRX — more than the \~6.5 TRX burned by a USDT transfer without rented Energy. Prices are live and change throughout the day.
{% endhint %}
