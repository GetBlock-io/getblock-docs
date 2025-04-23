---
description: >-
  Use the 'createwitness' method in Tron's HTTP REST API Interface to register a
  new witness node efficiently.
---

# createwitness - TRON

## Description

The 'createwitness' Web3 method in the Tron protocol allows users to register a new witness node through the createwitness HTTP REST API protocol. This function is crucial for those looking to participate in Tron’s delegated proof-of-stake consensus mechanism. By becoming a witness, users can help validate transactions and secure the network while earning rewards. The 'createwitness' method requires specific parameters, including the account address and other necessary credentials, to ensure a seamless registration process. Designed for developers and network participants, this method integrates smoothly with existing Web3 frameworks, offering a reliable and efficient way to expand and maintain the Tron network’s decentralized infrastructure.

## Supported Networks

The createwitness HTTP REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters createwitness method needs to be executed.

* **owner\_address** (required, string): The address of the owner for whom the witness is being created. This should be a valid blockchain address.
* **url** (required, string): The URL associated with the witness, typically encoded in hexadecimal format. This should be a valid URL representing the witness's information or website.
* **visible** (optional, boolean): A flag indicating whether the witness should be visible. The default value is `false`. If set to `true`, the witness will be publicly visible.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using createwitness

Request

```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/wallet/createwitness \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "owner_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
  "url": "007570646174654e616d6531353330363038383733343633",
  "visible": true
}
'
```

Response

```json
{
  "visible": true,
  "txID": "31ec6da41b7794ff85c12115a86deb19f67013272a8b2d6c8732b590cd48a08a",
  "raw_data": {
    "contract": [
      {
        "parameter": {
          "value": {
            "owner_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
            "url": "007570646174654e616d6531353330363038383733343633"
          },
          "type_url": "type.googleapis.com/protocol.WitnessCreateContract"
        },
        "type": "WitnessCreateContract"
      }
    ],
    "ref_block_bytes": "897a",
    "ref_block_hash": "eb8fc7d56ef59c35",
    "expiration": 1745338863000,
    "timestamp": 1745338805681
  },
  "raw_data_hex": "0a02897a2208eb8fc7d56ef59c3540989bb6f2e5325a83010805127f0a32747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5769746e657373437265617465436f6e747261637412490a154198927ffb9f554dc4a453c64b2e553a02d6df514b123030303735373036343631373436353465363136643635333133353333333033363330333833383337333333343336333370b1dbb2f2e532"
}
```

## Body Parameters

Here is the list of body parameters for the createwitness method:

1. **owner\_address**: The address of the account that is attempting to become a witness. This should be provided in a base58check encoded format.
2. **url**: A string containing the URL associated with the witness. This is typically used to provide more information about the witness, such as a website or contact details.
3. **Permission\_id**: Optional, for multi-signature use
4. **visible**: Optional, Whether the address is in base58 format.

These parameters are essential for successfully creating a witness on the Tron network and handling the response effectively.

## Use Case

Here are some use-cases for the `createwitness` method in Web3 programming:

1. **Decentralized Governance Participation**: The `createwitness` method can be utilized in blockchain networks that implement delegated proof-of-stake (DPoS) or similar consensus mechanisms. By creating a witness, a user can participate in the network's governance by validating transactions and blocks. This is crucial for maintaining the security and integrity of the blockchain. Users can also earn rewards for their participation, providing an incentive to contribute to the network's stability.
2. **Network Security Enhancement**: Another use case for the `createwitness` method is enhancing the security of a blockchain network. By increasing the number of witnesses, the network can achieve greater decentralization, making it more resilient against attacks. This is particularly important for new or growing blockchain projects that aim to build trust and reliability among their user base.
3. **Community Engagement and Reputation Building**: Creating a witness allows individuals or organizations to build a reputation within the blockchain community. By consistently performing their duties as a witness, they can gain trust and recognition, which may lead to increased influence and opportunities within the ecosystem. This can be particularly beneficial for developers or companies looking to establish themselves as key players in the Web3 space.

## Code for createwitness

```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/wallet/createwitness"

headers = {
    "accept": "application/json",
    "content-type": "application/json"
}

payload = {
    "owner_address": "TPswDDCAWhJAZGdHPidFg5nEf8TkNToDX1",
    "url": "007570646174654e616d6531353330363038383733343633",
    "visible": True
}

response = requests.post(url, json=payload, headers=headers)

print("Status Code:", response.status_code)
print("Response JSON:", response.json())
```

## Common Errors

Common Errors\
When using the createwitness HTTP REST API Tron method, the following issues may occur:

* Invalid owner address: Ensure the owner\_address field contains a valid Tron address. Double-check the address format and verify it against the Tron blockchain standards.
* Malformed URL: The URL field should be a valid hexadecimal string representing the encoded URL. Ensure the string is correctly encoded and follows the required format.
* Visibility flag mismatch: The visible field should be a boolean value. Confirm that it is set to either true or false and not a string or other data type.

The createwitness method is essential for establishing a witness node in the Tron network, allowing users to participate in the block validation process. This functionality enhances the decentralization and security of Web3 applications by enabling more nodes to contribute to the network's consensus mechanism.

### conclusion

The `createwitness` HTTP REST API method in Tron is essential for establishing a witness node, as demonstrated by the provided JSON data, which includes the owner's address and URL. This functionality is crucial for maintaining the network's decentralized governance and ensuring the security and efficiency of the Tron blockchain.
