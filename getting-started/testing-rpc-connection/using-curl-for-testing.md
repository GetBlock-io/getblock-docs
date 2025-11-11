---
description: >-
  These examples provide a starting point for testing your connection and
  querying blockchain data using cURL commands.
---

# Using cURL for testing

Before you start:

1. Create a **JSON-RPC** endpoint for the Ethereum blockchain from your GetBlock account.
2. Replace \<ACCESS\_TOKEN> in the examples below with your actual Access Token.

### Fetch the current block number

Run the following command to retrieve the latest block number:

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS_TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
}'
```

If successful, the response will include the current block number in hexadecimal value:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1449641"
}
```

### Get the chain ID

Identify the blockchain network with the `eth_chainId` method:

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS_TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
    "id": "getblock.io"
}'
```

Response example:

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x1"
}
```

In this example, `0x1` indicates the Ethereum Mainnet. The chain ID helps confirm which blockchain network you are interacting with.

### Check account balance by address

Retrieve the balance of an Ethereum address using `eth_getBalance`. Replace `<ACCOUNT_ADDRESS>` with the target wallet address:

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS_TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["<ACCOUNT_ADDRESS>", "latest"],
    "id": "getblock.io"
}'
```

Example response:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x5a70dac3910910"
}
```

The result field shows the account balance in wei (1 ether = 10¹⁸ wei).

{% hint style="info" %}
For a list of supported RPC methods with examples, navigate to [API Reference](../../guides/testing-rpc-connection/broken-reference/).
{% endhint %}
