---
description: >-
  Example code for the /v1/transactions/by_hash/{transaction_hash} json-rpc
  method. Сomplete guide on how to use
  /v1/transactions/by_hash/{transaction_hash} json-rpc in GetBlock.io Web3
  documentation.
---

# /v1/transactions/by\_hash/{transaction\_hash} - Aptos

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/v1/transactions/by_hash/0x0137b99014705df2077763263f170ebef40fc88006d09da3b9139a32d761b554' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "accumulator_root_hash": "0xb1a0103f04d8ef3a942f80f04d231e559ae9f2d2b5b1f04be510d64c1d0f1a8d",
    "changes": [],
    "event_root_hash": "0x414343554d554c41544f525f504c414345484f4c4445525f4841534800000000",
    "gas_used": "0",
    "hash": "0x0137b99014705df2077763263f170ebef40fc88006d09da3b9139a32d761b554",
    "state_change_hash": "0xafb6e14fe47d850fd0a7395bcfb997ffacf4715e0f895cc162c218e4a7564bc6",
    "state_checkpoint_hash": "0x4d1b411db4f596d3d5a44da27d165a89d2b2589cc56dac00e2d9430aa7999555",
    "success": true,
    "timestamp": "1685066477483995",
    "type": "state_checkpoint_transaction",
    "version": "147832590",
    "vm_status": "Executed successfully"
}
```
