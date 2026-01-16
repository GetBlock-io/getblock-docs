---
description: >-
  Example code for the net_listening JSON-RPC method. Complete guide on how to
  use net_listening JSON-RPC in GetBlock Web3 documentation
---

# net\_listening - Somnia

This method returns true if the client is actively listening for network connections on the Somnia network.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
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
{% code title="request.js" %}
```javascript
const axios = require('axios');

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'net_listening',
  params: []
};

axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', payload)
.then(response => console.log(response.data));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "net_listening",
    "params": []
}

response = requests.post("https://go.getblock.io/<ACCESS-TOKEN>/", json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": true
}
```
{% endcode %}

## Use Cases

* Node health checks
* Connection validation
* Monitoring node status
