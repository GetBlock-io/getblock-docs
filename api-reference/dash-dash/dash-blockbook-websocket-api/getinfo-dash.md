---
description: >-
  Example code for the getInfo WebSocket method. Complete guide on how to use
  the getInfo WebSocket method in the GetBlock Web3 documentation.
---

# getInfo - Dash

Returns the indexer status over WebSocket: coin name, best block height and hash, and backend version. Send a message with this method and match the response by its id.

## Parameters

{% hint style="info" %}
This method takes no parameters.
{% endhint %}

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "getInfo",
    "params": {}
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "name": "Dash",
        "shortcut": "DASH",
        "bestHeight": 2100000,
        "bestHash": "000000000000000abc12def34567890fedcba9876543210abcdef1234567890ff",
        "version": "0.4.0"
    }
}
```

## Response Fields

| Field      | Type   | Description               |
| ---------- | ------ | ------------------------- |
| bestHeight | number | Best indexed block height |
| bestHash   | string | Best indexed block hash   |
| shortcut   | string | Coin ticker (DASH)        |

## Use Cases

* **Connection Check**: Confirm the WebSocket is live and synced
* **Tip Reads**: Read the best height over WS
* **Diagnostics**: Report indexer status

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| error                     | WebSocket error | The request could not be served                   |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
