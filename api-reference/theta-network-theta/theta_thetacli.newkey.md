---
description: >-
  Example code for the thetacli.NewKey  {disallowed} json-rpc method. Сomplete
  guide on how to use thetacli.NewKey  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# thetacli.NewKey {disallowed} - Theta Network

#### Parameters

`password` - string

\-

T

h

e

p

a

s

s

w

o

r

d

f

o

r

t

h

e

n

e

w

a

c

c

o

u

n

t

.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "thetacli.NewKey",
"params": {},
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
