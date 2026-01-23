---
description: >-
  Example code for the net_listening JSON-RPC method. Complete guide on how to
  use net_listening JSON-RPC in GetBlock Web3 documentation.
---

# net\_listening - Celo

This method returns `true` if the client is actively listening for network connections on the Celo network.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "net_listening",
  "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="index.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'net_listening',
  params: []
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "net_listening",
    "params": []
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": true
}
```

## Response Definition

| Field  | Type    | Description         |
| ------ | ------- | ------------------- |
| result | boolean | `true` if listening |

## Use Cases

* Check node connectivity
* Health monitoring
* Network diagnostics

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32603     | Internal error |
