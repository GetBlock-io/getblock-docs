# filecoin\_chaingettipsetbyheight

This method returns the tipset at a given epoch. If the requested epoch has no blocks (a null round), the nearest earlier non-empty tipset is returned.

## Parameters

| Parameter | Type    | Required | Description                                             |
| --------- | ------- | -------- | ------------------------------------------------------- |
| epoch     | integer | Yes      | The chain epoch (height) to retrieve                    |
| tipsetKey | array   | Yes      | Tipset key to read from; empty array for the chain head |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.ChainGetTipSetByHeight", "params": [4523800, []], "id": "getblock.io"}'
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
        method: 'Filecoin.ChainGetTipSetByHeight',
        params: [4523800, []],
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
        'method': 'Filecoin.ChainGetTipSetByHeight',
        'params': [4523800, []],
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
        "Cids": [
            {
                "/": "bafy2bzacecdkonmhngylnnhrk4azkg2wkgcm6cnm5qn5sk4ww5cszjlvkgkd6"
            }
        ],
        "Blocks": [
            {
                "Miner": "f01234",
                "Height": 4523800
            }
        ],
        "Height": 4523800
    }
}
```

## Response Parameters

| Field  | Type    | Description                      |
| ------ | ------- | -------------------------------- |
| Cids   | array   | CIDs of the blocks in the tipset |
| Blocks | array   | Block headers of the tipset      |
| Height | integer | Epoch of the returned tipset     |

## Use Cases

* **Historical Reads**: Resolve a tipset at a specific epoch
* **Block Indexing**: Walk the chain epoch by epoch
* **Null Round Handling**: Get the nearest tipset when an epoch is empty
* **Snapshotting**: Pin a tipset key for consistent state queries

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
