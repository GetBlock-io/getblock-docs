---
description: >-
  Example code for the eth_getUncleCountByBlockHash json-rpc method. Сomplete
  guide on how to use eth_getUncleCountByBlockHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getUncleCountByBlockHash - Optimism

This method returns the number of uncle blocks in a specified block. On Optimism, this always returns 0 since the network doesn't have traditional uncle blocks

### Parameters

| Parameter | Type           | Description          | Required |
| --------- | -------------- | -------------------- | -------- |
| blockHash | DATA, 32 Bytes | The hash of a block. | Yes      |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
   "method": "eth_getUncleByBlockHashAndIndex",
  "params": [
    "0x3e4a6a9efc7e4db75269430a5683ae3c7505ab0848d7814bd1c73c76e6882467",
  ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
    "jsonrpc": "2.0",
     "method": "eth_getUncleByBlockHashAndIndex",
  "params": [
    "0x3e4a6a9efc7e4db75269430a5683ae3c7505ab0848d7814bd1c73c76e6882467"
  ],
    "id": "getblock.io"
};

let config = {
  method: "post",
  maxBodyLength: Infinity,
  url: "https://go.getblock.us/<ACCESS_TOKEN>",
  headers: {
    "Content-Type": "application/json",
  },
  data: data,
};

axios
  .request(config)
  .then((response) => {
    console.log(JSON.stringify(response.data));
  })
  .catch((error) => {
    console.log(error);
  });

```
{% endtab %}

{% tab title="Request" %}
```python
import requests
import json

url = "https://go.getblock.us/<ACESS_TOKEN>"

payload = json.dumps({
   "jsonrpc": "2.0",
     "method": "eth_getUncleByBlockHashAndIndex",
  "params": [
    "0x3e4a6a9efc7e4db75269430a5683ae3c7505ab0848d7814bd1c73c76e6882467"
  ],
    "id": "getblock.io"
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rs
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::builder()
        .build()?;

    let mut headers = reqwest::header::HeaderMap::new();
    headers.insert("Content-Type", "application/json".parse()?);

    let data = r#"{
   "jsonrpc": "2.0",
    "method": "eth_getUncleByBlockHashAndIndex",
  "params": [
    "0x3e4a6a9efc7e4db75269430a5683ae3c7505ab0848d7814bd1c73c76e6882467"
  ],
    "id": "getblock.io"
}"#;

    let json: serde_json::Value = serde_json::from_str(&data)?;

    let request = client.request(reqwest::Method::POST, "https://go.getblock.us/<ACCESS_TOKEN>")
        .headers(headers)
        .json(&json);

    let response = request.send().await?;
    let body = response.text().await?;

    println!("{}", body);

    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x0"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The uncle count as hexadecimal (always 0x0 on Optimism).

### Use Case

The eth\_getUncleCountByBlockHash method is commonly used for:

* **Ethereum compatibility**
* **Block analysis**

### Error handling

| Status Code | Error Message    | Cause                                                      |
| ----------- | ---------------- | ---------------------------------------------------------- |
| 404         | Not Found        | Missing or invalid ACCESS\_TOKEN.                          |
| -32602      | Invalid argument | <ul><li>Invalid block hash</li><li>Invalid index</li></ul> |
