# getnewshieldedaddress


## Meta Description
Access 'getnewshieldedaddress' using Tron’s RESTful API Interface for secure, private transactions.

## Description
The 'getnewshieldedaddress' Web3 method in the Tron protocol offers a secure way to generate new shielded addresses, ensuring user privacy in blockchain transactions. This method, accessible via Tron’s RESTful API Interface, is designed to facilitate private transactions by creating addresses that obscure transaction details from public visibility. Utilizing the 'getnewshieldedaddress' RPC protocol, developers can seamlessly integrate shielded address generation into their dApps, enhancing user confidentiality. The method is straightforward, providing a user-friendly interface for both novice and experienced developers to implement advanced privacy features in their applications. By leveraging this functionality, developers can ensure that their applications align with the growing demand for privacy in the blockchain space.

## Supported Networks
The getnewshieldedaddress REST API method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

Here is the list of parameters getnewshieldedaddress method needs to be executed.

- **owner_address** (required)
  - **Type**: String
  - **Description**: The address of the owner for whom the shielded address is being generated.
  - **Supported Values**: A valid address format, e.g., "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g".

- **first_token_id** (required)
  - **Type**: String
  - **Description**: The identifier of the first token.
  - **Supported Values**: A string representing the token ID, e.g., "31303030343837".

- **first_token_balance** (required)
  - **Type**: Integer
  - **Description**: The balance of the first token.
  - **Supported Values**: A non-negative integer, e.g., 100.

- **second_token_id** (required)
  - **Type**: String
  - **Description**: The identifier of the second token.
  - **Supported Values**: A string representing the token ID, e.g., "31303030303031".

- **second_token_balance** (required)
  - **Type**: Integer
  - **Description**: The balance of the second token.
  - **Supported Values**: A non-negative integer, e.g., 100.

- **visible** (optional)
  - **Type**: Boolean
  - **Description**: Determines if the shielded address should be visible.
  - **Default Value**: true
  - **Supported Values**: true or false.

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using getnewshieldedaddress

Request
```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/ \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "first_token_id": "31303030343837",
  "first_token_balance": 100,
  "second_token_id": "31303030303031",
  "second_token_balance": 100,
  "visible": true
}
```

Response
```json

{
  "sk": "0706d6a9d5c8b3dbdfcd8a45292a2657dcf3c17cd00d2f14a0cf20d14a626b50",
  "ask": "0c700aafdb56d91a6a570fea93193db345323deb9aa1e244b960769ee4aae805",
  "nsk": "064ea55760154757649a8b92c7ab744ef8ca199036ce0913748973d3333ab604",
  "ovk": "ffcf993fd44a75cd2dd69fe8d2a3c10bc301a9969171d891161a081af0abc4ab",
  "ak": "fddb839df1dbb46f8edc59878e7ddc39fb4ecba896fae45721b8d349604acc60",
  "nk": "b8b8e4eda1523eba1b6e8ea7822165b8b9990b881797f68fb7808355a53e1e03",
  "ivk": "2b9478fbfb97bfc658342376b994f6dd1a837840915378e1ba698bc91ea15705",
  "d": "40a45c508de910f671b721",
  "pkD": "27cc01832876e59eef7ddc0d8b2f288903c97efccfa72879c8ce6498144b439c",
  "payment_address": "ztron1gzj9c5ydayg0vudhyynucqvr9pmwt8h00hwqmze09zys8jt7ln86w2reer8xfxq5fdpecfhnmuy"
}
```
## Body Parameters

Here is the list of body parameters for the `getnewshieldedaddress` method:

1. **sk**: The spending key, a crucial component for authorizing transactions and managing shielded addresses.

2. **ask**: The ask parameter, part of the key structure used in shielded address operations.

3. **nsk**: This parameter is part of the nullifier key, which is used to detect double-spending attempts.

4. **ovk**: The outgoing viewing key, allowing the owner to view outgoing transactions.

5. **ak**: The address key, used for generating and managing the shielded address.

6. **nk**: Part of the nullifier key, ensuring the integrity and non-reusability of notes.

7. **ivk**: The incoming viewing key, which allows the owner to view incoming transactions.

8. **d**: A diversifier component used in the address generation process to create unique addresses.

9. **pkD**: The public key associated with the diversifier, used in the cryptographic operations of the address.

10. **payment_address**: The actual shielded address that can be used to receive funds in a private manner.

## Use Case

Here are some use-cases for the `getnewshieldedaddress` method in Web3 programming:

1. **Enhanced Privacy for Transactions**: One of the primary use-cases for the `getnewshieldedaddress` method is to enhance privacy in blockchain transactions. By generating a new shielded address, users can conduct transactions without exposing their public addresses, thereby maintaining anonymity. This is particularly useful in scenarios where privacy is paramount, such as in confidential business transactions or personal financial management.

2. **Secure Token Management**: In environments where multiple tokens are managed, the `getnewshieldedaddress` method can be used to segregate token holdings securely. For instance, if a user holds multiple types of tokens, creating separate shielded addresses for each token type can help in organizing and securing these assets. This approach minimizes the risk of exposure and potential loss if one address is compromised.

3. **Compliance with Regulatory Requirements**: In jurisdictions with strict privacy regulations, businesses can use the `getnewshieldedaddress` method to comply with legal requirements concerning data protection and privacy. By ensuring that transactions are conducted through shielded addresses, companies can demonstrate their commitment to protecting user data, thereby aligning with legal standards and building trust with their customers.

## Code for getnewshieldedaddress


```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "owner_address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "first_token_id": "31303030343837",
  "first_token_balance": 100,
  "second_token_id": "31303030303031",
  "second_token_balance": 100,
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```
## Common Errors

Common Errors  
When using the getnewshieldedaddress HTTP REST API Tron method, the following issues may occur:  
- **Invalid Owner Address**: If the owner address is not in the correct format or is invalid, the request will fail. Ensure the address follows the Tron address format and is correctly input.  
- **Insufficient Token Balance**: If the specified token balance exceeds the available balance, the operation will not proceed. Verify the token balances are adequate before making the request.  
- **Network Connectivity Issues**: Poor or unstable network connectivity can lead to timeouts or incomplete requests. Ensure a stable internet connection before attempting to use the API.  
- **Visibility Parameter Misconfiguration**: If the `visible` parameter is not correctly set, it may affect the address generation. Confirm that the visibility setting aligns with your intended privacy requirements.

Using the getnewshieldedaddress method in Web3 applications enhances privacy and security by generating shielded addresses for transactions. This functionality is crucial for maintaining confidentiality in blockchain interactions, providing users with the capability to conduct transactions without exposing sensitive information.

### conclusion

The provided data showcases a wallet with specific token balances and visibility settings. By utilizing the getnewshieldedaddress HTTP API on the Tron network, users can enhance their privacy and security. This API facilitates the creation of new shielded addresses, offering a more confidential transaction experience on the blockchain.
