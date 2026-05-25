# is\_key\_image\_spent monero

This non-JSON-RPC endpoint checks whether one or more key images have been spent. Key images are unique identifiers used to prevent double-spends in Monero's ring-signature scheme.

## Parameters

| Parameter   | Type            | Required | Description               |
| ----------- | --------------- | -------- | ------------------------- |
| key\_images | array of string | Yes      | Key images (hex) to check |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/is_key_image_spent' \
--header 'Content-Type: application/json' \
--data-raw '{
    "key_images": [
        "8d1bd8181bf7d857bdb281e0153d84cd55a3fcaa57c3e570f4a49f935850b5e3"
    ]
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "key_images": [
        "8d1bd8181bf7d857bdb281e0153d84cd55a3fcaa57c3e570f4a49f935850b5e3"
    ]
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/is_key_image_spent',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/is_key_image_spent"

payload = json.dumps({
    "key_images": [
        "8d1bd8181bf7d857bdb281e0153d84cd55a3fcaa57c3e570f4a49f935850b5e3"
    ]
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "key_images": [
                "8d1bd8181bf7d857bdb281e0153d84cd55a3fcaa57c3e570f4a49f935850b5e3"
        ]
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/is_key_image_spent")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "spent_status": [
        1
    ],
    "status": "OK",
    "untrusted": false
}
```

## Response Parameters

| Field         | Type                  | Description                                                               |
| ------------- | --------------------- | ------------------------------------------------------------------------- |
| spent\_status | array of unsigned int | Per-key-image: `0` = unspent, `1` = spent on-chain, `2` = seen in mempool |
| status        | string                | `OK` on success                                                           |

## Use Cases

* Wallet reconciliation after restoring from seed
* Verifying outputs are still spendable
* Auditing key-image uniqueness in research contexts

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
// Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
// Use a plain HTTP client as shown in the Request Example above.
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/is_key_image_spent', {"key_images": ["8d1bd8181bf7d857bdb281e0153d84cd55a3fcaa57c3e570f4a49f935850b5e3"]});
console.log(response.data);
```
{% endtab %}

{% tab title="monero-python" %}
```python
# Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
# Use a plain HTTP client as shown in the Request Example above.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/is_key_image_spent', json={"key_images": ["8d1bd8181bf7d857bdb281e0153d84cd55a3fcaa57c3e570f4a49f935850b5e3"]})
print(response.json())
```
{% endtab %}
{% endtabs %}
