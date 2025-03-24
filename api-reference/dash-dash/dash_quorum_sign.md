---
description: >-
  Example code for the quorum_sign  {disallowed} json-rpc method. Сomplete guide
  on how to use quorum_sign  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# quorum\_sign {disallowed} - Dash

#### Parameters

`submethod` - string

None

`llmqType` - number

Type of quorums to list:

\- 1 - LLMQ\_50\_60

\- 2 - LLMQ\_400\_60

\- 3 - LLMQ\_400\_85

\- 4 - LLMQ\_100\_67

`id` - string (hex)

Signing request ID. Signing request ids for ChainLocks and InstantSend are calculated as described in:

\- The ChainLocks DIP

\- The LLMQ InstantSend DIP.

For general signing requests, any 32 byte hex string can be provided as the request id. Note that if a quorum hash is not specified in parameter 4, a quorum will be selected automatically based in part on this value.

`msgHash` - string (hex)

Hash of the message to be signed.

`quorumHash` - string (hex)

Optional.

The quorum identifier.

`submit` - bool

Optional.

Added in Dash Core 0.17.0

Submits the signature share to the network if this is true (default). Returns an object containing the signature share if this is false.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "quorum",
"params": ["sign", null, null, null, null, null],
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
