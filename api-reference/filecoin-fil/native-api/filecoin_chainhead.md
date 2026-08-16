---
description: >-
  Example code for the Filecoin.ChainHead JSON RPC method. Complete guide on how
  to use Filecoin.ChainHead JSON RPC in GetBlock Web3 documentation.
---

# Filecoin.ChainHead - Filecoin

This method returns the current head of the chain: the tipset at the latest epoch, including its block CIDs, block headers, and height.

## Parameters

This method does not accept any parameters. Pass an empty array.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.ChainHead", "params": [], "id": "getblock.io"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        jsonrpc: '2.0',
        method: 'Filecoin.ChainHead',
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'Filecoin.ChainHead',
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
        "Cids": [
            {
                "/": "bafy2bzacecdkonmhngylnnhrk4azkg2wkgcm6cnm5qn5sk4ww5cszjlvkgkd6"
            },
            {
                "/": "bafy2bzaceaglcpzhd5gfrzdyt7ce3e5asnbfz3s3stbqyxniziny5snewbpbg"
            }
        ],
        "Blocks": [
            {
                "Miner": "f01234",
                "Height": 4523891
            }
        ],
        "Height": 4523891
    }
}
```

## Response Parameters

| Field  | Type    | Description                                                      |
| ------ | ------- | ---------------------------------------------------------------- |
| Cids   | array   | CIDs of the blocks in the tipset, each as an object with a / key |
| Blocks | array   | Block headers of the tipset                                      |
| Height | integer | Epoch (block height) of the tipset                               |

## Use Cases

* **Tip Tracking**: Read the current chain height and tipset CIDs
* **Sync Checks**: Compare the head against a local index
* **Polling**: Detect new tipsets by watching the height
* **State Anchoring**: Use the head tipset key for state reads

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
