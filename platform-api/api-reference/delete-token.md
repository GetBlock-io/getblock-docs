---
description: >-
  Delete a GetBlock access token. Its endpoint URL stops working immediately.
  Complete guide to DELETE /tokens/{id} in the Platform API.
---

# Delete access token - Platform API

`DELETE /tokens/{id}` deletes one of your tokens, whatever its access type. The token stops working immediately.

{% code overflow="wrap" %}
```
DELETE https://public-api.getblock.io/api/v1/tokens/{id}
```
{% endcode %}

{% hint style="danger" %}
**This cannot be undone.** Every client still using the token's endpoint URL starts failing. To replace a leaked token with a working one, [rotate](rotate-token.md) it instead.
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The token's `id`. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X DELETE https://public-api.getblock.io/api/v1/tokens/9b2796d7b4c448de83b01644bb93f625 \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const tokenId = '9b2796d7b4c448de83b01644bb93f625';

const response = await fetch(`https://public-api.getblock.io/api/v1/tokens/${tokenId}`, {
  method: 'DELETE',
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});

console.log(response.status); // 204
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

token_id = "9b2796d7b4c448de83b01644bb93f625"

response = requests.delete(
    f"https://public-api.getblock.io/api/v1/tokens/{token_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
)

print(response.status_code)  # 204
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

`204 No Content`, without a body.

## Response Parameters

None.

## Use Cases

* Remove endpoints of a service you shut down
* Free a slot when you reach the plan's token limit
* Revoke access for a customer who left your service

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 404 | No such token among yours, or it is already deleted. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
