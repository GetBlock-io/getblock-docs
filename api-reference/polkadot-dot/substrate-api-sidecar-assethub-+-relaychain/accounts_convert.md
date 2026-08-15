---
description: >-
  Example code for the /accounts/{accountId}/convert REST method. Complete guide
  on how to use /accounts/{accountId}/convert REST in GetBlock Web3
  documentation.
---

# /accounts/{accountId}/convert - Polkadot

This endpoint converts an account address between SS58 formats and its raw AccountId, using the given prefix and scheme.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/accounts/{accountId}/convert`).
{% endhint %}

## Endpoint

```http
GET /accounts/{accountId}/convert
```

## Path Parameters

| Parameter | Type   | Description                |
| --------- | ------ | -------------------------- |
| accountId | string | SS58 address or public key |

## Query Parameters

| Parameter | Type    | Required | Description                                |
| --------- | ------- | -------- | ------------------------------------------ |
| scheme    | string  | No       | Signature scheme (sr25519, ed25519, ecdsa) |
| prefix    | integer | No       | Target SS58 prefix (0 for Polkadot)        |
| publicKey | boolean | No       | Whether the input is a public key          |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/convert?prefix=0'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/convert?prefix=0');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/convert?prefix=0')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "ss58Prefix": "0",
  "network": "polkadot",
  "address": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5",
  "accountId": "0xd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d",
  "scheme": "sr25519",
  "publicKey": true
}
```

## Response Fields

| Field      | Type   | Description                            |
| ---------- | ------ | -------------------------------------- |
| ss58Prefix | string | The SS58 prefix used                   |
| network    | string | The network name for the prefix        |
| address    | string | The SS58-encoded address               |
| accountId  | string | The raw 32-byte AccountId, hex-encoded |

## Use Cases

* **Address Conversion**: Convert an address between chain prefixes
* **Cross-Chain UX**: Show the same account across networks
* **Key Handling**: Derive the AccountId from an address
* **Indexing**: Normalize addresses to a canonical form
