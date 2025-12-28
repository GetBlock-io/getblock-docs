---
description: >-
  Example code for the help JSON-RPC method. Complete guide on how to use help
  JSON-RPC in GetBlock Web3 documentation.
---

# help - Bitcoin

This method lists all available commands or provides help for a specified command.

### Parameters

| Parameter | Type   | Required | Description                                                  |
| --------- | ------ | -------- | ------------------------------------------------------------ |
| command   | string | No       | The command to get help for. If omitted, lists all commands. |

### Request

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "help",
   "params": ["getblock"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```jsx
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "help",
    "params": ["getblock"],
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
{% endtab %}

{% tab title="Request" %}
```py
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "help",
    "params": ["getblock"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust" %}
```rs
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "help",
            "params": ["getblock"],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

### Response

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "getblock \"blockhash\" ( verbosity )\n\nIf verbosity is 0, returns a string that is serialized, hex-encoded data for block 'hash'.\nIf verbosity is 1, returns an Object with information about block <hash>.\nIf verbosity is 2, returns an Object with information about block <hash> and information about each transaction.\n\nArguments:\n1. blockhash    (string, required) The block hash\n2. verbosity    (numeric, optional, default=1) 0 for hex-encoded data, 1 for a JSON object, 2 for JSON object with transaction data\n\nResult (for verbosity = 0):\n\"hex\"    (string) A string that is serialized, hex-encoded data for block 'hash'\n\n..."
}
```
{% endcode %}

### Response Parameters

| Field  | Type   | Description                                                  |
| ------ | ------ | ------------------------------------------------------------ |
| result | string | Help text for the specified command or list of all commands. |

### Use Case

The `help` method is essential for:

* Discovering available RPC commands
* Learning command parameters and usage
* Building dynamic documentation
* Debugging API integration
* Exploring node capabilities
* Creating command reference tools

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration With Web3

The `help` method helps developers:

* Explore available RPC methods
* Understand parameter requirements
* Build self-documenting tools
* Create API exploration interfaces
* Support developer onboarding
