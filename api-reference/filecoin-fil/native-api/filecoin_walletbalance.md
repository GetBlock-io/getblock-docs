# filecoin\_walletbalance

This method returns the balance of an address in attoFIL. It reads the balance directly from chain state and does not require the address to be in the node's wallet.

## Parameters

| Parameter | Type   | Required | Description                        |
| --------- | ------ | -------- | ---------------------------------- |
| address   | string | Yes      | The address to read the balance of |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.WalletBalance", "params": ["f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq"], "id": "getblock.io"}'
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
        method: 'Filecoin.WalletBalance',
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
        'method': 'Filecoin.WalletBalance',
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
    "result": "5000000000000000000"
}
```

## Response Parameters

| Field  | Type   | Description                                            |
| ------ | ------ | ------------------------------------------------------ |
| result | string | The address balance in attoFIL (1 FIL = 10^18 attoFIL) |

## Use Cases

* **Balance Display**: Show an address's FIL balance
* **Payment Checks**: Confirm an address holds enough FIL
* **Accounting**: Read balances for reconciliation
* **Monitoring**: Watch an address balance over time

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
