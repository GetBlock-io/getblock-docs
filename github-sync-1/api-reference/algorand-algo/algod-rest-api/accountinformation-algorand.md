---
description: >-
  Example code for the AccountInformation REST method. Complete guide on how to
  use AccountInformation REST in GetBlock Web3 documentation.
---

# AccountInformation - Algorand

Given a specific account public key, this call returns the account's status, balance and spendable amounts.

## Endpoint

```http
GET /v2/accounts/{address}
```

## Path Parameters

| Parameter | Type   | Required | Description            |
| --------- | ------ | -------- | ---------------------- |
| address   | string | Yes      | An account public key. |

## Query Parameters

| Parameter | Type   | Required | Description                                                                                                                                                      |
| --------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| exclude   | array  | Optional | Exclude additional items from the account. Use `all` to exclude asset holdings, application local state, created asset parameters, and created application param |
| format    | string | Optional | Configures whether the response object is JSON or MessagePack encoded. If not provided, defaults to JSON.                                                        |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/accounts/REPLACE_ADDRESS"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field                          | Type    | Description                                                                                                                                                     |
| ------------------------------ | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| address                        | string  | the account public key                                                                                                                                          |
| amount                         | integer | \[algo] total number of MicroAlgos in the account                                                                                                               |
| amount-without-pending-rewards | integer | specifies the amount of MicroAlgos in the account, without the pending rewards.                                                                                 |
| apps-local-state               | array   | \[appl] applications local data stored in this account.                                                                                                         |
| apps-total-extra-pages         | integer | \[teap] the sum of all extra application program pages for this account.                                                                                        |
| apps-total-schema              | object  | Specifies maximums on the number of each type that may be stored.                                                                                               |
| assets                         | array   | \[asset] assets held by this account.                                                                                                                           |
| auth-addr                      | string  | \[spend] the address against which signing should be checked. If empty, the address of the current account is used. This field can be updated in any transactio |
| created-apps                   | array   | \[appp] parameters of applications created by this account including app global data.                                                                           |
| created-assets                 | array   | \[apar] parameters of assets created by this account.                                                                                                           |
| incentive-eligible             | boolean | Whether or not the account can receive block incentives if its balance is in range at proposal time.                                                            |
| last-heartbeat                 | integer | The round in which this account last went online, or explicitly renewed their online status.                                                                    |
| last-proposed                  | integer | The round in which this account last proposed the block.                                                                                                        |
| min-balance                    | integer | MicroAlgo balance required by the account.                                                                                                                      |
| participation                  | object  | AccountParticipation describes the parameters used by this account in consensus protocol.                                                                       |
| pending-rewards                | integer | amount of MicroAlgos of pending rewards in this account.                                                                                                        |
| reward-base                    | integer | \[ebase] used as part of the rewards computation. Only applicable to accounts which are participating.                                                          |
| rewards                        | integer | \[ern] total rewards of MicroAlgos the account has received, including pending rewards.                                                                         |
| round                          | integer | The round for which this information is relevant.                                                                                                               |
| sig-type                       | string  | Indicates what type of signature is used by this account, must be one of:                                                                                       |
| status                         | string  | \[onl] delegation status of the account's MicroAlgos                                                                                                            |
| total-apps-opted-in            | integer | The count of all applications that have been opted in, equivalent to the count of application local data (AppLocalState objects) stored in this account.        |
| total-assets-opted-in          | integer | The count of all assets that have been opted in, equivalent to the count of AssetHolding objects held by this account.                                          |
| total-box-bytes                | integer | \[tbxb] The total number of bytes used by this account's app's box keys and values.                                                                             |
| total-boxes                    | integer | \[tbx] The number of existing boxes created by this account's app.                                                                                              |
| total-created-apps             | integer | The count of all apps (AppParams objects) created by this account.                                                                                              |
| total-created-assets           | integer | The count of all assets (AssetParams objects) created by this account.                                                                                          |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
