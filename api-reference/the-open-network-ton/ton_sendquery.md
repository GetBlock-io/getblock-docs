---
description: >-
  Example code for the /sendQuery json-rpc method. Сomplete guide on how to use
  /sendQuery json-rpc in GetBlock.io Web3 documentation.
---

# /sendQuery - The Open Network (TON)

#### Parameters

`address` - body

string, required

address in any format

`body` - body

string, required

b64-encoded boc-serialized cell with message body

`init_code` - body

string, optional

b64-encoded boc-serialized cell with init-code

`init_data` - body

string, optional

b64-encoded boc-serialized cell with init-data

#### Request

```java
curl --location --request POST 'https://ton.getblock.io/mainnet/rest/sendQuery?' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{}'
```
