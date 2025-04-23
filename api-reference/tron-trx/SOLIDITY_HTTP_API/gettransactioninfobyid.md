---
description: >-
  Retrieve Tron transaction details with 'gettransactioninfobyid' via the HTTP
  REST API Interface. Fast, reliable data access.
---

# gettransactioninfobyid - TRON

## Description

The gettransactioninfobyid method in the Tron protocol allows developers to fetch detailed transaction information using a transaction ID. This HTTP REST API endpoint is essential for querying blockchain data, supporting both gettransactioninfobyid Web3 integrations and direct 'gettransactioninfobyid RPC protocol' calls. It returns critical details like block height, timestamp, contract triggers, and fee breakdowns, making it ideal for analytics, auditing, and dApp functionality. A key tool for Tron developers, it ensures seamless, programmatic access to on-chain transaction metadata.

## Supported Networks

The gettransactioninfobyid HTTP REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

#### _value_

* **Type**: `string`
* **Description**:\
  The transaction hash, which is the same as the transaction ID.\
  Default value: `d0807adb3c5412aa150787b944c96ee898c997debdc27e2f6a643c771edb5933`.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using gettransactioninfobyid

Request

```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' 
--header 'Content-Type: application/json' 
{
  "value": "d0807adb3c5412aa150787b944c96ee898c997debdc27e2f6a643c771edb5933"
}
```

Response

```json
{}
```

## Body Parameters



* **id** (`string`):\
  The transaction ID.
* **fee** (`int64`):\
  Total amount of TRX burned in the transaction, including bandwidth and energy fees, memo fee, account activation fee, multi-signature fee, and other fees.
* **blockNumber** (`int64`):\
  The block number containing this transaction.
* **blockTimeStamp** (`int64`):\
  The timestamp of the block, measured in milliseconds.
* **contractResult** (`string[]`):\
  An array of results from the execution of smart contract calls within the transaction.
* **contract\_address** (`string`):\
  The address of the smart contract called.
* **receipt** (`ResourceReceipt`):\
  Receipt of the transaction, including detailed execution results and fees:
  * **energy\_usage**: Energy consumed from the caller’s account.
  * **energy\_fee**: TRX burned for energy consumption.
  * **origin\_energy\_usage**: Energy consumed from the contract deployer’s account.
  * **energy\_usage\_total**: Total energy used by the transaction.
  * **net\_usage**: Bandwidth consumed.
  * **net\_fee**: TRX burned for bandwidth usage.
  * **result**: Transaction execution result.
  * **energy\_penalty\_total**: Extra energy fee due to calling popular contracts.
* **log** (`Log[]`):\
  Events triggered during the smart contract execution. Each log entry includes:
  * **address**: Contract address (note: TVM addresses are hex without `0x41` prefix; add `41` and convert to Base58 format if needed).
  * **topics**: Indexed event parameters.
  * **data**: Non-indexed event parameters.
* **result** (`int`):\
  Execution result. If successful, this field is omitted; if failed, it will display `"FAILED"`.
* **resMessage** (`string`):\
  Detailed error message if the transaction failed. Encoded in hex; can be converted to plaintext.
* **withdraw\_amount** (`int64`):\
  For withdrawal reward or unfreeze transactions, this field shows the amount of rewards withdrawn to the account (unit: sun).
* **unfreeze\_amount** (`int64`):\
  In Stake1.0, for unstaking transactions, this field shows the amount of TRX unstaked (unit: sun).
* **internal\_transactions** (`InternalTransaction[]`):\
  Internal transactions triggered by the execution of the main transaction.
* **withdraw\_expire\_amount** (`int64`):\
  In Stake2.0, for unstaking, withdrawing unfrozen balance, or cancelling all unstakes, this field shows the amount of TRX withdrawn to the account (unit: sun).
* **cancel\_unfreezeV2\_amount** (`map<string, int64>`):\
  Amount of TRX re-staked to obtain resources. The key can be:
  * `"BANDWIDTH"`
  * `"ENERGY"`
  * `"TRON_POWER"`\
    The value represents the unstaked principal canceled (unit: sun).

## Use Case

Here are some use-cases for the `gettransactioninfobyid` method in Web3 programming:

1. **Transaction Verification and Status Tracking**\
   Developers can use this method to verify the details and status of a specific transaction (e.g., whether it was successfully mined, reverted, or is still pending). By passing the transaction hash (like the one in your example), they can retrieve metadata such as block confirmation count, gas used, and execution status, which is crucial for auditing and debugging smart contract interactions.
2. **Fee Analysis and Optimization**\
   The method helps analyze transaction costs by providing details like gas consumption and effective gas price. This is useful for optimizing smart contract interactions—developers can compare historical transactions to adjust gas limits or pricing strategies for future transactions, reducing costs.
3. **Event Log Retrieval for DApps**\
   Decentralized applications (DApps) often rely on transaction logs to trigger UI updates or backend processes. By querying a transaction’s info, developers can extract emitted events (e.g., ERC-20 transfers or NFT trades) and use them to update user dashboards or synchronize off-chain databases.

This method is particularly valuable for transparency and real-time monitoring in blockchain applications.

## Code for gettransactioninfobyid

```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "value": "d0807adb3c5412aa150787b944c96ee898c997debdc27e2f6a643c771edb5933"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```

## Common Errors

**Common Errors**\
When using the `gettransactioninfobyid` REST Tron method, the following issues may occur:

* **Invalid transaction ID format**: The provided hash is malformed or incorrect. Ensure the ID is a 64-character hexadecimal string (e.g., `d0807adb3c5412aa150787b944c96ee898c997debdc27e2f6a643c771edb5933`).
* **Transaction not found**: The ID does not correspond to any existing transaction on the Tron network. Verify the ID or check if the transaction was broadcast successfully.
* **Node synchronization issues**: The node may not be fully synced with the Tron blockchain, leading to missing data. Use a reliable, up-to-date node or retry later.
* **Rate limiting or timeout**: High request volume or network latency can cause failures. Implement retry logic or adjust timeout settings in your application.

The `gettransactioninfobyid` method is essential for Web3 apps to fetch detailed transaction data, enabling transparency and auditability. By leveraging this RPC call, developers can verify transaction statuses, confirmations, and smart contract interactions efficiently.

### conclusion

The provided HTTP contains a transaction hash value, which can be used with the **gettransactioninfobyid REST** method on the **Tron** blockchain to retrieve detailed transaction information. By querying this endpoint, users can obtain metadata such as block height, contract details, and execution status, making it a powerful tool for transaction analysis. Integrating **gettransactioninfobyid** into your workflow ensures efficient tracking and verification of transactions on the **Tron** network.
