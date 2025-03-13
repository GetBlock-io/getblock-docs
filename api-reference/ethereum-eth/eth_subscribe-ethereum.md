---
description: >-
  The eth_subscribe method is part of the Ethereum JSON-RPC API and is used to
  subscribe to specified event types. Available only via WebSocket
---

# eth\_subscribe-Ethereum

{% hint style="success" %}
Subscribes to a specified event type, optionally restricted to one ormany objects. This method is available via websocket only.
{% endhint %}

The eth\_subscribe method is part of the Ethereum JSON-RPC API and is used to subscribe to specified event types. This method is available only via WebSocket connections and allows clients to receive real-time updates about blockchain events.

### Supported Networks

The eth\_subscribe RPC Ethereum method is supported on:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

The method accepts the following parameters:

type (string): A subscription type. Possible values include:

* newHeads: Subscribes to new headers appended to the chain, including chain reorganizations.
* logs: Subscribes to logs included in new imported blocks that match the specified filter criteria.
* newPendingTransactions: Subscribes to transaction hashes for all transactions added to the pending state and signed with a key available in the node.

objects (optional, hex string): Additional arguments such as an address, multiple addresses, or topics for filtering logs or events.

### Request

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To interact with the Ethereum eth\_subscribe endpoint using WebSocket, use the following example:

{% tabs %}
{% tab title="ws" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_subscribe",
"params": ["newHeads", null],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The response may indicate success or an error, depending on the availability of WebSocket notifications.

Response

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": "0xe5af64ddfd365b4632988c5935cfedb7"
}
```

### Response Description

* result: On success, returns the subscription ID for the requested event type.
* eth\_subscribe error: On failure, provides error details, such as "notifications not supported."
* value: The details of subscribed event updates, depending on the subscription type, such as block headers, logs, or transaction hashes.

### Use Case

The eth\_subscribe RPC Ethereum method is commonly used in Web3 applications and monitoring tools to:

* Track new block headers for real-time blockchain synchronization.
* Monitor specific contract logs using eth\_subscribe logs to capture events emitted by smart contracts.
* Observe new pending transactions for analytics or operational insights.

For example, a Web3 application can use the Ethereum eth\_subscribe method to provide live updates about blockchain activity, enhancing user interaction and real-time feedback.

### Code Example

Here is an eth\_subscribe example of how to query the method using JavaScript:

{% tabs %}
{% tab title="Python" %}
```python
import websocket
import json

# Replace YOUR-API-KEY with your API key
API_KEY = "YOUR-API-KEY"
URL = f"wss://eth.getblock.io/{API_KEY}/"

def on_message(ws, message):
    print("Message received:", message)

def on_error(ws, error):
    print("Error:", error)

def on_close(ws, close_status_code, close_msg):
    print("Connection closed")

def on_open(ws):
    # Request body to subscribe to new block headers
    request_body = {
        "jsonrpc": "2.0",
        "method": "eth_subscribe",
        "params": ["newHeads", None],
        "id": "getblock.io"
    }
    ws.send(json.dumps(request_body))
    print("Request sent:", request_body)

if __name__ == "__main__":
    websocket.enableTrace(True)
    ws = websocket.WebSocketApp(URL, 
                                on_message=on_message, 
                                on_error=on_error, 
                                on_close=on_close)
    ws.on_open = on_open
    ws.run_forever()
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const WebSocket = require('ws');

// Replace YOUR-API-KEY with your API key
const API_KEY = 'YOUR-API-KEY';
const URL = `wss://eth.getblock.io/${API_KEY}/`;

// Connect to the WebSocket server
const ws = new WebSocket(URL);

// Event: Connection opened
ws.on('open', function open() {
  const requestBody = {
    jsonrpc: '2.0',
    method: 'eth_subscribe',
    params: ['newHeads', null],
    id: 'getblock.io',
  };

  ws.send(JSON.stringify(requestBody));
  console.log('Request sent:', requestBody);
});

// Event: Message received
ws.on('message', function incoming(data) {
  console.log('Message received:', data);
});

// Event: Error occurred
ws.on('error', function error(err) {
  console.error('Error:', err);
});

// Event: Connection closed
ws.on('close', function close() {
  console.log('Connection closed');
});
```
{% endtab %}
{% endtabs %}

#### Common Errors

When using the eth\_subscribe RPC Ethereum method, the following issues may occur:

* Invalid URL or ACCESS-TOKEN: Ensure the WebSocket URL and token are correct and active.
* eth\_subscribe error: This could happen if the requested event type or parameters are invalid.
* Unsupported Notifications: If WebSocket notifications are not enabled on the node, the error "notifications not supported" may be returned.

By integrating the Web3 eth\_subscribe method into your application, you can provide real-time updates for blockchain events. Use this core API method to enhance user experiences and enable live monitoring of Ethereum network activities.
