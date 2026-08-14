# filecoin\_statenetworkversion

This method returns the Filecoin network protocol version in effect at a given tipset. The network version increments with each network upgrade.

{% hint style="info" %}
Many state methods take a tipset key as their final parameter: an array of block CIDs identifying the tipset to read. Pass an empty array `[]` to read against the current chain head.
{% endhint %}

## Parameters

| Parameter | Type  | Required | Description                                             |
| --------- | ----- | -------- | ------------------------------------------------------- |
| tipsetKey | array | Yes      | Tipset key to read from; empty array for the chain head |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.StateNetworkVersion", "params": [[]], "id": "getblock.io"}'
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
        method: 'Filecoin.StateNetworkVersion',
        params: [[]],
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
        'method': 'Filecoin.StateNetworkVersion',
        'params': [[]],
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
    "result": 23
}
```

## Response Parameters

| Field  | Type    | Description                                |
| ------ | ------- | ------------------------------------------ |
| result | integer | The network protocol version at the tipset |

## Use Cases

* **Upgrade Detection**: Detect network upgrades by version
* **Compatibility**: Adapt message construction to the network version
* **Tooling**: Configure SDKs against the active version
* **Diagnostics**: Log the network version for support

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
