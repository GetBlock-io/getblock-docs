# filecoin\_version

This method returns the version of the node software, its API version, and the block delay (epoch duration) of the network.

## Parameters

This method does not accept any parameters. Pass an empty array.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.Version", "params": [], "id": "getblock.io"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        jsonrpc: '2.0',
        method: 'Filecoin.Version',
        params: [],
        id: 'getblock.io'
    })
});

const data = await response.json();
console.log(data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'Filecoin.Version',
        'params': [],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "Version": "1.28.0+mainnet",
        "APIVersion": 131840,
        "BlockDelay": 30
    }
}
```

## Response Parameters

| Field      | Type    | Description                               |
| ---------- | ------- | ----------------------------------------- |
| Version    | string  | Node software version string              |
| APIVersion | integer | Encoded node API version                  |
| BlockDelay | integer | Epoch duration in seconds (30 on mainnet) |

## Use Cases

* **Node Diagnostics**: Identify the node version
* **Compatibility Checks**: Confirm the API version is supported
* **Timing**: Read the block delay for epoch-based logic
* **Support Reports**: Include the node version in a report

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
