# node\_version

This endpoint returns the node's client version, implementation name, and the chain it is connected to.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/node/version`).
{% endhint %}

## Endpoint

```http
GET /node/version
```

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/node/version'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/node/version');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/node/version')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "clientVersion": "1.15.0-a1b2c3d",
  "clientImplName": "parity-polkadot",
  "chain": "Polkadot"
}
```

## Response Fields

| Field          | Type   | Description                |
| -------------- | ------ | -------------------------- |
| clientVersion  | string | Node client version        |
| clientImplName | string | Client implementation name |
| chain          | string | Connected chain name       |

## Use Cases

* **Diagnostics**: Identify the node version and chain
* **Compatibility**: Confirm a supported client release
* **Monitoring**: Track versions across nodes
* **Support**: Include node details in a report
