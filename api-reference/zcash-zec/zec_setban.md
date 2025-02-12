---
description: >-
  Example code for the setban  {disallowed} json-rpc method. Сomplete guide on
  how to use setban  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# setban {disallowed} - Zcash

#### Parameters

`ip(/netmask)` - string

The IP/Subnet (see getpeerinfo for nodes IP) with an optional netmask (default is /32 = single IP)

`command` - string

"add" to add an IP/Subnet to the list, "remove" to remove an IP/Subnet from the list.

`bantime` - numeric

Optional.

time in seconds how long (or until when if \[absolute] is set) the IP is banned (0 or empty means using the default time of 24h which can also be overwritten by the -bantime startup argument)

`absolute` - boolean

Optional.

If set, the bantime must be an absolute timestamp in seconds since epoch (Jan 1 1970 GMT)

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
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
