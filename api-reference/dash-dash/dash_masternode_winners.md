---
description: >-
  Example code for the masternode_winners json-rpc method. Сomplete guide on how
  to use masternode_winners json-rpc in GetBlock.io Web3 documentation.
---

# masternode\_winners - Dash

#### Parameters

`method name` - string

None

`count` - string (hex)

Optional

Number of previous block winners to display (default: 10)

`filter` - string

Optional

Payment address to filter by

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "masternode",
"params": ["winners", null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "1535904": "XrckquhAFHPiq2xAhEcxc89Phn1phD59Mi",
        "1535905": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535906": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535907": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535908": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535909": "XdYqyxCnCd9ewYNf4kbX4hRJaixN421SUm",
        "1535910": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535911": "Xrbh7thf5UK7UnCr57kNpLwxkSrFn91dcC",
        "1535912": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535913": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535914": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535915": "XshfZjCw2Ar1vT7uSpHWCmtF1pE7pvJt4c",
        "1535916": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535917": "XtLJ85td8RTe2584GnJngL27KYyZqEEX1R",
        "1535918": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535919": "Xefyiv1TmqnpZofgTbNWUkma1nmCHaA7Hh",
        "1535920": "XdYqyxCnCd9ewYNf4kbX4hRJaixN421SUm",
        "1535921": "Xepq4w7bbFRqDDoE7cR1gmsbm9EpfW8r4y",
        "1535922": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535923": "XqLKENxFQ27Sqg8t46jSht6bnatWaZiMcj",
        "1535924": "XwfiDpB29rbeW3kwsNvG5NFYGfDa5dQbyE",
        "1535925": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535926": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535927": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535928": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535929": "XcMRwEWiWzhtdJSJALbqAafd9iU1peU7b6",
        "1535930": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535931": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535932": "Xn757SZsKRprKqQhv6up51DuXcYoFtp47R",
        "1535933": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7",
        "1535934": "XoDo3w4ZUqn93uL3FeRNb9fgfXvgg7BvDC, Xnem6ejaXAfKDTh5PFGSa9ozMQNMY6bvs7"
    }
}
```
