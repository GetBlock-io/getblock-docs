---
description: >-
  Example code for the setban  {disallowed} json-rpc method. Сomplete guide on
  how to use setban  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# setban {disallowed} - Bitcoin Gold

#### Parameters

`subnet` - string, required

The IP/Subnet (see getpeerinfo for nodes IP) with an optional netmask (default is /32 = single IP)

`command` - string, required

‘add’ to add an IP/Subnet to the list, ‘remove’ to remove an IP/Subnet from the list

`bantime` - numeric, optional, default=0

time in seconds how long (or until when if \[absolute] is set) the IP is banned (0 or empty means using the default time of 24h which can also be overwritten by the -bantime startup argument)

`absolute` - boolean, optional, default=false

If set, the bantime must be an absolute timestamp expressed in UNIX epoch time)

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
