---
description: >-
  Example code for the /transaction/material REST method. Complete guide on how
  to use /transaction/material REST in GetBlock Web3 documentation.
---

# /transaction/material - Polkadot

This endpoint returns the data needed to construct and sign a transaction offline: the genesis hash, chain name, spec and transaction versions, and metadata.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/transaction/material`).
{% endhint %}

## Endpoint

```http
GET /transaction/material
```

## Query Parameters

| Parameter       | Type    | Required | Description                               |
| --------------- | ------- | -------- | ----------------------------------------- |
| noMeta          | boolean | No       | Omit the metadata field from the response |
| metadataVersion | integer | No       | Metadata version to include               |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/transaction/material'
```
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/transaction/material');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/transaction/material')
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
  "genesisHash": "0x91b171bb158e2d3848fa23a9f1c25182fb8e20313b2c1eb49219da7a70ce90c3",
  "chainName": "Polkadot",
  "specName": "polkadot",
  "specVersion": "1002000",
  "txVersion": "26",
  "metadata": "0x6d657461..."
}
```

## Response Fields

| Field       | Type   | Description                         |
| ----------- | ------ | ----------------------------------- |
| genesisHash | string | Genesis block hash                  |
| chainName   | string | Chain name                          |
| specVersion | string | Runtime spec version                |
| txVersion   | string | Transaction format version          |
| metadata    | string | SCALE-encoded metadata, hex-encoded |

## Use Cases

* **Offline Signing**: Gather signing material for cold wallets
* **Transaction Construction**: Build extrinsics with correct versions
* **Custody Systems**: Prepare transactions for secure signing
* **SDK Integration**: Feed signing material to a client
