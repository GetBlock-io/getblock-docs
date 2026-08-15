# accounts\_balance info

This endpoint returns the balance details for an account: free, reserved, and frozen amounts, the nonce, and any balance locks, at the latest or a specified block.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/accounts/{accountId}/balance-info`).
{% endhint %}

## Endpoint

```http
GET /accounts/{accountId}/balance-info
```

## Path Parameters

| Parameter | Type   | Description          |
| --------- | ------ | -------------------- |
| accountId | string | SS58 account address |

## Query Parameters

| Parameter | Type   | Required | Description                                                    |
| --------- | ------ | -------- | -------------------------------------------------------------- |
| at        | string | No       | Block height or hash to query at. Defaults to the latest block |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/balance-info'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/balance-info');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/balance-info')
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
  "nonce": "1234",
  "tokenSymbol": "DOT",
  "free": "50000000000",
  "reserved": "1000000000",
  "frozen": "0",
  "locks": []
}
```

## Response Fields

| Field       | Type   | Description                |
| ----------- | ------ | -------------------------- |
| nonce       | string | Account nonce              |
| tokenSymbol | string | Native token symbol (DOT)  |
| free        | string | Free balance in Planck     |
| reserved    | string | Reserved balance in Planck |
| frozen      | string | Frozen balance in Planck   |
| locks       | array  | Active balance locks       |

## Use Cases

* **Balance Display**: Show an account's spendable and reserved balance
* **Wallets**: Read balance and nonce in a single call
* **Accounting**: Reconcile free, reserved, and frozen amounts
* **Transfer Checks**: Confirm sufficient free balance before a transfer
