---
description: >-
  Example code for the Filecoin.ChainGetBlock JSON RPC method. Complete guide on
  how to use Filecoin.ChainGetBlock JSON RPC in GetBlock Web3 documentation.
---

# Filecoin.ChainGetBlock - Filecoin

This method returns the block header for a given block CID, including its miner, parents, height, and message roots.

## Parameters

| Parameter | Type   | Required | Description                          |
| --------- | ------ | -------- | ------------------------------------ |
| cid       | object | Yes      | Block CID, as an object with a / key |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.ChainGetBlock", "params": [{"/": "bafy2bzacecdkonmhngylnnhrk4azkg2wkgcm6cnm5qn5sk4ww5cszjlvkgkd6"}], "id": "getblock.io"}'
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
        method: 'Filecoin.ChainGetBlock',
        params: [{"/": "bafy2bzacecdkonmhngylnnhrk4azkg2wkgcm6cnm5qn5sk4ww5cszjlvkgkd6"}],
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
        'method': 'Filecoin.ChainGetBlock',
        'params': [{"/": "bafy2bzacecdkonmhngylnnhrk4azkg2wkgcm6cnm5qn5sk4ww5cszjlvkgkd6"}],
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
        "Miner": "f01234",
        "Height": 4523891,
        "ParentWeight": "123456789",
        "Parents": [
            {
                "/": "bafy2bzaceaglcpzhd5gfrzdyt7ce3e5asnbfz3s3stbqyxniziny5snewbpbg"
            }
        ],
        "ParentStateRoot": {
            "/": "bafy2bzaceaglcpzhd5gfrzdyt7ce3e5asnbfz3s3stbqyxniziny5snewbpbg"
        },
        "Messages": {
            "/": "bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"
        },
        "Timestamp": 1719400000
    }
}
```

## Response Parameters

| Field           | Type    | Description                                  |
| --------------- | ------- | -------------------------------------------- |
| Miner           | string  | Address of the miner that produced the block |
| Height          | integer | Epoch of the block                           |
| Parents         | array   | CIDs of the parent tipset's blocks           |
| ParentStateRoot | object  | CID of the parent state root                 |
| Messages        | object  | CID of the block's message root              |
| Timestamp       | integer | Unix timestamp of the block                  |

## Use Cases

* **Block Explorers**: Render a block header by CID
* **Chain Traversal**: Follow parent CIDs up the chain
* **State Roots**: Read the parent state root for state queries
* **Producer Analytics**: Attribute blocks to miners

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
