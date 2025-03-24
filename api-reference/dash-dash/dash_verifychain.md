---
description: >-
  Example code for the verifychain json-rpc method. Сomplete guide on how to use
  verifychain json-rpc in GetBlock.io Web3 documentation.
---

# verifychain - Dash

#### Parameters

`Check Level` - number (int)

Optional.

How thoroughly to check each block, from 0 to 4.

Default is the level set with the -checklevel command line argument; if that isn't set, the default is 3.

Levels are:

\- 0. Read from disk to ensure the files are accessible

\- 1. Ensure each block is valid

\- 2. Make sure undo files can be read from disk and are in a valid format

\- 3. Test each block undo to ensure it results in correct state

\- 4. After undoing blocks, reconnect them to ensure they reconnect correctly

`Number Of Blocks` - number (int)

Optional.

The number of blocks to verify. Set to 0 to check all blocks.

Defaults to the value of the -checkblocks command-line argument; if that isn't set, the default is 288.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "verifychain",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": true
}
```
