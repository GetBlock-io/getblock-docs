---
description: >-
  Example code for the setban  {disallowed} json-rpc method. Сomplete guide on
  how to use setban  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# setban {disallowed} - Dash

#### Parameters

`IP(/Netmask)` - string

The node to add or remove as a string in the form of IP\_address.

The IP address may be a hostname resolvable through DNS, an IPv4 address, an IPv4-as-IPv6 address, or an IPv6 address.

`command` - string

What to do with the IP/Subnet address above.

Options are:

\- add to add a node to the addnode list

\- remove to remove a node from the list. If currently connected, this will disconnect immediately

`Bantime` - numeric (int)

Optional.

Time in seconds how long (or until when if absolute is set) the entry is banned. The default is 24h which can also be overwritten by the -bantime startup argument.

`Absolute` - bool

Optional.

If set, the bantime must be a absolute timestamp in seconds since epoch (Jan 1 1970 GMT)

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "setban",
"params": [null, null, null, null],
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
