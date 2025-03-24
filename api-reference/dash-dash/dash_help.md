---
description: >-
  Example code for the help json-rpc method. Сomplete guide on how to use help
  json-rpc in GetBlock.io Web3 documentation.
---

# help - Dash

#### Parameters

`RPC` - string

Optional.

The name of the RPC to get help for. If omitted, Dash Core 0.10x will display an alphabetical list of commands; Dash Core 0.11.0 will display a categorized list of commands.

`Sub-command` - string

Optional.

The subcommand to get help on. Please note that not all subcommands support this at the moment.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "help",
"params": ["help", null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "List all commands, or get help for a specified command"
}
```
