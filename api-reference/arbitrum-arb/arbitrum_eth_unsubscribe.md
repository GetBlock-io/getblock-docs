---
description: >-
  Example code for the eth_unsubscribe JSON RPC method. Сomplete guide on how to
  use eth_unsubscribe JSON RPC in GetBlock Web3 documentation.
---

# eth\_unsubscribe - Arbitrum

This method allows developers to cancel a subscription by its subscription ID

{% hint style="info" %}
This method is only available through a WebSocket endpoint.
{% endhint %}

#### Parameters

| Field           | Type   | Required | Description                                   |
| --------------- | ------ | -------- | --------------------------------------------- |
| subscription ID | string | Yes      | The identifier of the subscription to cancel. |

#### Request

{% hint style="success" %}
Note that subscriptions require a WebSocket connection and [WebSocket cat](https://www.npmjs.com/package/wscat) for you to use this method in the console.Install WebSocket cat with:`npm install -g wscat`
{% endhint %}

{% tabs %}
{% tab title="WSCAT" %}
```bash
$ wscat -c wss://go.getblock.us/<ACCESS_TOKEN>
# Wait for the connection to be established

Connected (press CTRL+C to quit)

> {"id":1,"jsonrpc":"2.0","method":"eth_unsubscribe","params":["0x9c7c3f3c3c63b1b89ac0c1e0b0d3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8090a1b"]}
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
import WebSocket from ('ws');

const webSocket = new WebSocket('wss://go.getblock.us/<ACCESS_TOKEN>');

async function subscribeToNewBlocks() {

  const request = {
    id: 1,
    jsonrpc: '2.0',
    method: 'eth_subscribe',
    params: ['0x9c7c3f3c3c63b1b89ac0c1e0b0d3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8090a1b'],
  };

  const onOpen = (event) => {
    webSocket.send(JSON.stringify(request));
  };

  const onMessage = (event) => {
    const response = JSON.parse(event.data);
    console.log(response);
  };

  try {
    webSocket.addEventListener('open', onOpen);
    webSocket.addEventListener('message', onMessage);
  } catch (error) {
    console.error(error);
  }
}

subscribeToNewBlocks();
```
{% endtab %}
{% endtabs %}

#### Response

```java
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": true
}
```

#### Reponse Parameter Definition

| Field name | Data type | Description                                                              |
| ---------- | --------- | ------------------------------------------------------------------------ |
| result     | boolean   | A boolean indicating whether the subscription was successfully canceled. |

#### Error handling

| Status Code | Error Message                            | Cause                                                        |
| ----------- | ---------------------------------------- | ------------------------------------------------------------ |
| 403         | Forbidden                                | Missing or invalid ACCESS\_TOKEN.                            |
| -32000      | <ul><li>subrsciption not found</li></ul> | <ul><li>the subcription ID is missing or incorrect</li></ul> |

#### Integration with Web3

The `eth_unsubscribe` helps developers to:

* Manage live event listeners more efficiently in Web3 frontends.
* Prevent unnecessary loads by stopping subscriptions when components unmount.
* Improve the performance of dashboards that need to toggle between live and static views.
* Support mobile dApps where bandwidth consumption must remain low.
* Dynamically pause or resume event streaming depending on user actions.
