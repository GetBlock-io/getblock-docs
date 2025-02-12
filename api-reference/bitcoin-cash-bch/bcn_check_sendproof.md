---
description: >-
  Example code for the check_sendproof json-rpc method. Сomplete guide on how to
  use check_sendproof json-rpc in GetBlock.io Web3 documentation.
---

# check\_sendproof - Bitcoin Cash

#### Parameters

`sendproof` - string

A sendproof to check.

#### Request

```java
curl --location --request POST 'https://bcn.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "check_sendproof",
"params": {"sendproof": "bcn1PRoof1R1qSQZb2f7qU4j9dPWkRhaK19GsNhUQxJPiEk2W5fjLdEZxBiKf8FsT67T1M2qk9zx33eEKMAtqLPLRzGtLyVhtMvbVAoTcLJmgn6QEvKH16XRAKeHobfr2piBHy9neyNe1WcTk5cPNhvaJjcNPhyzdQW4s3pfLxuj11htCn4ggDwsT1rBFqJeTQoQbSS3aNiUbpq2Raa1pydVWxHZN7Hz2Kt6ZGgcYZ6MiVLWrnXeVAAtGVuvJE3tU6J3JKr1aoZjtg2bPH5DimtTUhPwj1SwTwGGijYuv8rXQLSs7SuaiAJh3WbYyvhSCidQPPz44wMvMsS5eWtWK9DkYniUfzPoJr9zDkyS5z8A2V3BTjboHyeiHW6bSRyGWDy9gShFN6ExGzVdW2ZiF9GfP3M9GT2UXzZ4LP7jNK5opHmQcu3uAVbg53zXHy7AFKqAtvzns3n3mNY67AgGxxRfUQ99Fak77UMCGWqCoheGbSoJV7PFwjexNcEdBctQAVn931ik7MoawSdtdtExQafc2Tc6GqsLL1Pm5UcWugy1uBqVuYPjKXnzKJfX7L8h794bzBiXQUP5pNyrcEGYgZQ9Mfu6dGZuWksPBYMo9JrdsARPaL"},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "transaction_hash": "fe331d318ffa931647cec4d046e93c00da53944e63a134f68a1ba4a3501411c5",
        "address": "2AREy1c2nBj7NVWB3SNnVDhLEN8Jve1UQ6FQYJdn9BLsQeDaUdAoGTr5tD9zn7XouYErdGafrHzfQfNEGt9XyDPB9jdzoc5",
        "amount": 10000,
        "message": "Luck I\u2019m your father!",
        "output_indexes": [
            0
        ]
    }
}
```
