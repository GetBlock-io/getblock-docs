---
description: >-
  Example code for the addnode  {disallowed} json-rpc method. Сomplete guide on
  how to use addnode  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# addnode {disallowed} - Dash

#### Parameters

`node` - string

The node to add as a string in the form of IP\_address:port.

`command` - string

What to do with the IP address above. Options are:

\- add to add a node to the addnode list. Up to 8 nodes can be added additional to the default 8 nodes. Not limited by -maxconnections.

\- remove to remove a node from the list. If currently connected, this will disconnect immediately

\- onetry to immediately attempt connection to the node even if the outgoing connection slots are full; this will only attempt the connection once.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "addnode",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
