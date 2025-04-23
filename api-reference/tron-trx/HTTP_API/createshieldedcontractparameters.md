---
description: >-
  Create shielded contract parameters using the
  'createshieldedcontractparameters' method via Tron’s RESTful API Interface.
---

# createshieldedcontractparameters - TRON

## Description

The 'createshieldedcontractparameters' Web3 method in the Tron protocol provides developers with a powerful tool to generate shielded contract parameters, ensuring enhanced privacy and security for smart contracts. This method is accessible through the Tron RESTful API Interface, offering a seamless integration path for developers working with decentralized applications. By leveraging the 'createshieldedcontractparameters' RPC protocol, users can efficiently initiate shielded transactions, protecting transaction details from public scrutiny. This method is essential for developers aiming to implement privacy-focused features in their applications, ensuring that sensitive contract data remains confidential. The technical design of this API method ensures user-friendly interaction while maintaining robust security standards.

## Supported Networks

The createshieldedcontractparameters REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

{% hint style="info" %}
This method does **not require any parameters**
{% endhint %}

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using createshieldedcontractparameters

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/createshieldedcontractparameters \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
```

Response

```json

{
  "Error": "class org.tron.core.exception.ContractValidateException : No valid shielded TRC-20 contract address"
}
```

## Body Parameters

* **ovk** (string, required)
  * **Description:** Outgoing viewing key. It is used to allow third parties to view transaction details without exposing private keys. Essential for enabling transparency when needed.
* **from\_amount** (string, required)
  * **Description:** The amount of the shielded TRC20 token being sent from the sender's shielded balance. It should be provided as a string representing the numeric value.
* **to\_amount** (string, required)
  * **Description:** The amount intended to be transferred to the recipient. Must match the token denomination and format required by the shielded contract.
* **transparent\_to\_address** (string, required)
  * **Description:** The TRON base58 address of the recipient if the recipient is a transparent account (non-shielded). If transferring to another shielded address, this field might not be used.
* **shielded\_TRC20\_contract\_address** (string, required)
  * **Description:** The address of the shielded TRC20 smart contract handling the privacy transaction. It must be provided in a correct TRON address format.
* **ask** (string, required)
  * **Description:** Authorizing spending key. It is a part of the cryptographic key set allowing the holder to spend funds from the shielded address securely.
* **nsk** (string, required)
  * **Description:** Nullifier deriving key. It is used to generate nullifiers for spent notes, ensuring no double-spending occurs in shielded transactions.

## Use Case

Here are some use-cases for the `createshieldedcontractparameters` method in Web3 programming:

1. **Privacy-Preserving Transactions**: One of the primary use-cases for the `createshieldedcontractparameters` method is to facilitate privacy-preserving transactions on blockchain platforms. By creating shielded contract parameters, developers can ensure that sensitive details of a transaction, such as the amount being transferred and the parties involved, remain confidential. This is particularly useful in scenarios where financial privacy is paramount, such as in decentralized finance (DeFi) applications and private token transfers.
2. **Secure Token Exchanges**: Another use-case is in the context of secure token exchanges. When users want to exchange tokens on a decentralized exchange (DEX) without revealing their transaction details to the public, the `createshieldedcontractparameters` method can be employed. By generating shielded parameters, users can conduct exchanges while maintaining privacy, thus protecting themselves from potential front-running attacks or other forms of market manipulation.
3. **Compliance with Regulatory Requirements**: In certain jurisdictions, there may be regulatory requirements that mandate the protection of user data and transaction details. The `createshieldedcontractparameters` method can be used to comply with these regulations by ensuring that sensitive information is not exposed on the public ledger. This is particularly relevant for institutions that need to adhere to privacy laws such as the General Data Protection Regulation (GDPR) while still leveraging blockchain technology for their operations.

## Code for createshieldedcontractparameters

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>//wallet/createshieldedcontractparameters"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "exchange_id": 12,
  "token_id": "31303030343837",
  "quant": 100,
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

Common Errors\
When using the createshieldedcontractparameters HTTP REST API Tron method, the following issues may occur:

* Invalid Owner Address: The provided owner address may not be in the correct format or may not exist on the Tron network. Ensure the address is a valid TRON address and double-check for typographical errors.
* Incorrect Exchange ID: The exchange\_id might not correspond to a valid exchange on the network. Verify the exchange ID is correct and corresponds to the intended exchange by cross-referencing with official documentation or network resources.
* Token ID Mismatch: The token\_id must match an existing token on the Tron network. Ensure the token ID is correctly formatted and corresponds to a token that exists and is active.
* Insufficient Quant Value: The quant field must represent a valid and sufficient quantity of tokens for the transaction. Confirm the quant value is appropriate and that the owner address has enough balance to cover the transaction.

The createshieldedcontractparameters method in Web3 applications provides a powerful tool for creating shielded contracts, enhancing privacy and security on the Tron network. By leveraging this functionality, developers can offer users a more secure and confidential transaction experience, which is increasingly important in decentralized applications.

### conclusion

The provided JSON snippet showcases the parameters required for the `createshieldedcontractparameters` HTTP API on the Tron network, including details such as the `owner_address`, `exchange_id`, `token_id`, `quant`, and visibility status. This API facilitates the creation of shielded contracts, ensuring enhanced privacy and security for transactions on the Tron blockchain. By leveraging the `createshieldedcontractparameters`, users can effectively manage and execute private transactions with confidentiality.
