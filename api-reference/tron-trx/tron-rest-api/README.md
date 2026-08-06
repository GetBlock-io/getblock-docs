---
description: >-
  REST API reference for TRON blockchain. Explore method list, request examples,
  and how to connect to GetBlock's TRON RPC endpoints
---

# Tron REST API

The TRON REST (HTTP) API is TRON's native interface. Every endpoint is an HTTP `POST` to `/wallet/{method}` on the Fullnode, with a JSON body and a JSON response. Read endpoints are also served by the Solidity node at `/walletsolidity/{method}`, which returns only confirmed, irreversible data — use it for balance and payment verification.

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/wallet/{method}
```
{% endtab %}

{% tab title="New York, USA" %}
{% code overflow="wrap" %}
```bash
https://shared.us-east-1.getblock.io/<ACCESS_TOKEN/wallet/{method}
```
{% endcode %}
{% endtab %}

{% tab title="Singapore, Singapore" %}
{% code overflow="wrap" %}
```bash
https://shared.ap-southeast-1.getblock.io/<ACCESS_TOKEN/wallet/{method}
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. Addresses may be supplied in base58 (`T...`) form by setting `visible` to true, or in hex (`41...`) form otherwise. TRX amounts are in SUN.
{% endhint %}

## Account

| Method             | Description                                |
| ------------------ | ------------------------------------------ |
| getaccount         | Account data, balance, permissions, assets |
| getaccountresource | Energy and Bandwidth resource state        |
| getaccountnet      | Bandwidth (network) state                  |

## Block

| Method              | Description       |
| ------------------- | ----------------- |
| getnowblock         | The latest block  |
| getblockbynum       | A block by height |
| getblockbylimitnext | A range of blocks |

## Transaction

| Method                 | Description                        |
| ---------------------- | ---------------------------------- |
| gettransactionbyid     | A transaction by id                |
| gettransactioninfobyid | Execution result, energy, and logs |
| createtransaction      | Build an unsigned TRX transfer     |
| broadcasttransaction   | Broadcast a signed transaction     |

## Resources (Stake 2.0)

| Method            | Description                            |
| ----------------- | -------------------------------------- |
| freezebalancev2   | Stake TRX for Energy or Bandwidth      |
| unfreezebalancev2 | Unstake TRX                            |
| delegateresource  | Delegate a resource to another account |

## Smart Contract

| Method                  | Description                              |
| ----------------------- | ---------------------------------------- |
| triggerconstantcontract | Read-only contract call (e.g. balanceOf) |
| triggersmartcontract    | Build a state-changing contract call     |
| getcontract             | Contract metadata and ABI                |
| estimateenergy          | Estimate Energy for a call               |

## Chain

| Method             | Description                     |
| ------------------ | ------------------------------- |
| getchainparameters | On-chain governance parameters  |
| getnodeinfo        | Node diagnostics and sync state |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [TRON Official HTTP API Reference](https://developers.tron.network/reference/full-node-api-overview)
