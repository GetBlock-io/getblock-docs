# runtime\_metadata

This endpoint returns the runtime metadata as decoded JSON. The metadata version can be selected with the version query parameter.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/runtime/metadata`).
{% endhint %}

## Endpoint

```http
GET /runtime/metadata
```

## Query Parameters

| Parameter | Type    | Required | Description                                        |
| --------- | ------- | -------- | -------------------------------------------------- |
| version   | integer | No       | Metadata version to return (for example, 14 or 15) |
| at        | string  | No       | Block height or hash to query at                   |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/runtime/metadata?version=15'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/runtime/metadata?version=15');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/runtime/metadata?version=15')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "magicNumber": 1635018093,
  "metadata": {
    "v15": {
      "pallets": [
        {
          "name": "System",
          "index": 0
        },
        {
          "name": "Balances",
          "index": 5
        }
      ]
    }
  }
}
```

## Response Fields

| Field       | Type    | Description                                |
| ----------- | ------- | ------------------------------------------ |
| magicNumber | integer | Metadata magic number                      |
| metadata    | object  | Decoded runtime metadata, keyed by version |

## Use Cases

* **SDK Bootstrapping**: Load decoded metadata without SCALE parsing
* **Pallet Discovery**: Enumerate pallets, calls, and storage
* **Type Registries**: Build type registries from metadata
* **Tooling**: Generate typed bindings from metadata
