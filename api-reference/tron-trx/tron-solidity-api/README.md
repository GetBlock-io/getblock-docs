# Tron Solidity API

The Solidity node exposes a query-only subset of the native TRON HTTP API that returns only confirmed, irreversible data. It is the correct interface for balance and payment verification: reading from the Fullnode can surface transactions that are later rolled back, while the Solidity node returns only finalized state. Every endpoint is an HTTP `POST` to `/walletsolidity/{method}`.

Request and response shapes match the Fullnode endpoint of the same name; the difference is data freshness. The Solidity node serves reads only — transaction creation, broadcasting, and staking are Fullnode operations.

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/{method}
```
{% endtab %}

{% tab title="New York, USA" %}
{% code overflow="wrap" %}
```bash
https://shared.us-east-1.getblock.io/<ACCESS_TOKEN/walletsolidity/{method}
```
{% endcode %}
{% endtab %}

{% tab title="Singapore, Singapore" %}
{% code overflow="wrap" %}
```bash
https://shared.ap-southeast-1.getblock.io/<ACCESS_TOKEN/walletsolidity/{method}
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
RReplace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. Addresses may be supplied in base58 (`T...`) form by setting `visible` to true, or in hex (`41...`) form otherwise. TRX amounts are in SUN.
{% endhint %}

{% hint style="info" %}
The most common "missing transaction" error comes from reading the Fullnode (`/wallet`) when the Solidity node (`/walletsolidity`) should be used. For anything that must be final — confirmed balances, settled payments, irreversible receipts — read from the Solidity node.
{% endhint %}

## Account

| Method     | Description                                          |
| ---------- | ---------------------------------------------------- |
| getaccount | Confirmed account data, balance, permissions, assets |

## Block

| Method              | Description                      |
| ------------------- | -------------------------------- |
| getnowblock         | The latest confirmed block       |
| getblockbynum       | A confirmed block by height      |
| getblockbyid        | A confirmed block by hash        |
| getblockbylatestnum | The most recent confirmed blocks |
| getblockbylimitnext | A range of confirmed blocks      |

## Transaction

| Method                        | Description                                  |
| ----------------------------- | -------------------------------------------- |
| gettransactionbyid            | A confirmed transaction by id                |
| gettransactioninfobyid        | Confirmed execution result, energy, and logs |
| gettransactioninfobyblocknum  | All transaction results in a confirmed block |
| gettransactioncountbyblocknum | Transaction count in a confirmed block       |

## Smart Contract

| Method                  | Description                                     |
| ----------------------- | ----------------------------------------------- |
| triggerconstantcontract | Read-only contract call against confirmed state |
| estimateenergy          | Estimate Energy against confirmed state         |

## Resources & Rewards

| Method                 | Description                                      |
| ---------------------- | ------------------------------------------------ |
| getdelegatedresourcev2 | Confirmed Stake 2.0 delegations between accounts |
| getreward              | Confirmed unclaimed voting and staking rewards   |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [TRON Official Solidity Node HTTP API](https://developers.tron.network/reference/getaccount-1)
* [REST API (Fullnode)](../tron-rest-api/)
