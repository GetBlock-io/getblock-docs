# filecoin\_stateaccountkey

This method returns the public-key address (f1 secp256k1 or f3 BLS) associated with an account actor's ID address at a tipset.

{% hint style="info" %}
Many state methods take a tipset key as their final parameter: an array of block CIDs identifying the tipset to read. Pass an empty array `[]` to read against the current chain head.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | The ID address to resolve (f0...)                       |
| tipsetKey | array  | Yes      | Tipset key to read from; empty array for the chain head |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.StateAccountKey", "params": ["f0123456", []], "id": "getblock.io"}'
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
        method: 'Filecoin.StateAccountKey',
        params: ["f0123456", []],
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
        'method': 'Filecoin.StateAccountKey',
        'params': ["f0123456", []],
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
    "result": "f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq"
}
```

## Response Parameters

| Field  | Type   | Description                                       |
| ------ | ------ | ------------------------------------------------- |
| result | string | The public-key address (f1 or f3) for the account |

## Use Cases

* **Key Recovery**: Resolve an ID address to its public-key address
* **Signature Verification**: Read the key address that must sign for an account
* **Wallet Display**: Show a user their public-key address
* **Address Mapping**: Map between ID and key address forms

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
