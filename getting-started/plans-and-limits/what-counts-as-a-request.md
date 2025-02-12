---
description: >-
  Any API call to a blockchain node through GetBlock is treated as one request
  regardless of its complexity.
---

# What counts as a request

Every request is counted equally: GetBlock deducts one request from your account balance for each API call. This makes it easier to track and plan your usage.

### What is a request?

Any operation that communicates with a blockchain node through GetBlock’s infrastructure is called a request. It is essentially an API call used to send or receive data on the blockchain.

These requests are how your software – a wallet or a dApp – communicates with blockchain nodes.

For instance, you can retrieve the details of a block by its hash, check an account balance using its address, send a transaction, call a smart contract method, and so on.
