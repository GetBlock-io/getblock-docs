---
description: >-
  Example code for the /pallets/{palletId}/storage/{storageItemId} REST method.
  Complete guide on how to use/pallets/{palletId}/storage/{storageItemId} REST
  in GetBlock Web3 documentation.
---

# /pallets/{palletId}/storage/{storageItemId} - Polkadot

This endpoint returns the decoded value of a storage item in a pallet, resolving the required keys. It removes the need to compute storage keys or decode SCALE manually.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/pallets/{palletId}/storage/{storageItemId}`).
{% endhint %}

## Endpoint

```http
GET /pallets/{palletId}/storage/{storageItemId}
```

## Path Parameters

| Parameter     | Type   | Description                           |
| ------------- | ------ | ------------------------------------- |
| palletId      | string | Pallet name or index, such as staking |
| storageItemId | string | Storage item name, such as ledger     |

## Query Parameters

| Parameter | Type   | Required | Description                              |
| --------- | ------ | -------- | ---------------------------------------- |
| keys\[]   | array  | No       | Keys for map or double-map storage items |
| at        | string | No       | Block height or hash to query at         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/pallets/staking/storage/ledger?keys[]=15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/pallets/staking/storage/ledger?keys[]=15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/pallets/staking/storage/ledger?keys[]=15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5')
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
  "pallet": "staking",
  "palletIndex": "7",
  "storageItem": "ledger",
  "keys": [
    "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5"
  ],
  "value": {
    "stash": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5",
    "total": "100000000000",
    "active": "100000000000"
  }
}
```

## Response Fields

| Field       | Type   | Description                         |
| ----------- | ------ | ----------------------------------- |
| pallet      | string | Pallet name                         |
| storageItem | string | Storage item name                   |
| keys        | array  | Resolved keys used to read the item |
| value       | object | The decoded storage value           |

## Use Cases

* **Decoded Reads**: Read pallet storage without SCALE decoding
* **Staking Data**: Read a nominator's ledger
* **Governance**: Read referenda or preimage storage
* **Custom Pallets**: Read parachain-specific storage items
