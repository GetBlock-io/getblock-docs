# filecoin\_mpoolgetnonce

This method returns the next nonce for an address, accounting for messages already in the message pool. It is used to construct the next message from a sender.

## Parameters

| Parameter | Type   | Required | Description        |
| --------- | ------ | -------- | ------------------ |
| address   | string | Yes      | The sender address |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.MpoolGetNonce", "params": ["f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq"], "id": "getblock.io"}'
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
        method: 'Filecoin.MpoolGetNonce',
        params: ["f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq"],
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
        'method': 'Filecoin.MpoolGetNonce',
        'params': ["f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq"],
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
    "result": 43
}
```

## Response Parameters

| Field  | Type    | Description                          |
| ------ | ------- | ------------------------------------ |
| result | integer | The next nonce to use for the sender |

## Use Cases

* **Message Construction**: Set the nonce for a new message
* **Sequencing**: Order multiple messages from one sender
* **Resubmission**: Replace a stuck message at the same nonce
* **Wallet Backends**: Compute nonces server-side before signing

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
