# runtime\_spec

This endpoint returns the runtime specification: the spec name and version, transaction version, and chain properties such as token symbol and decimals.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/runtime/spec`).
{% endhint %}

## Endpoint

```http
GET /runtime/spec
```

## Query Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| at        | string | No       | Block height or hash to query at |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/runtime/spec'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/runtime/spec');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/runtime/spec')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "at": {
    "hash": "0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32",
    "height": "6754362"
  },
  "authoringVersion": "0",
  "transactionVersion": "26",
  "implVersion": "0",
  "specName": "polkadot",
  "specVersion": "1002000",
  "chainType": {
    "live": null
  },
  "properties": {
    "ss58Format": "0",
    "tokenDecimals": [
      "10"
    ],
    "tokenSymbol": [
      "DOT"
    ]
  }
}
```

## Response Fields

| Field              | Type   | Description                                      |
| ------------------ | ------ | ------------------------------------------------ |
| specName           | string | Runtime spec name (polkadot)                     |
| specVersion        | string | Runtime spec version                             |
| transactionVersion | string | Transaction format version                       |
| properties         | object | Chain properties (SS58 format, decimals, symbol) |

## Use Cases

* **Upgrade Detection**: Detect runtime upgrades by specVersion
* **Transaction Signing**: Read the transaction version
* **Chain Config**: Read token decimals and SS58 format
* **Tooling**: Configure SDKs against the runtime
