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
        "network": "DASH",
        "decimals": 8,
        "version": "unknown",
        "bestHeight": 2540675,
        "bestHash": "000000000000000344a8914b400c4d5e572ba8e1c25388f012104ec7d9b81b99",
        "block0Hash": "00000ffd590b1485b3caadc19b22e6379c733355108f107a430458cdf3407ab6",
        "testnet": false,
        "backend": {
            "version": "230108",
            "subversion": "/Dash Core:23.1.8/"
        }
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
