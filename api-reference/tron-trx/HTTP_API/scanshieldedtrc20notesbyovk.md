---
description: >-
  ScanShieldedTrc20NotesByOvk method retrieves shielded TRC-20 notes by OVK on
  the TRON blockchain
---

# scanshieldedtrc20notesbyovk - TRON

## Description

The ScanShieldedTrc20NotesByOvk Web3 method within the Tron protocol offers a robust solution for scanning shielded TRC-20 notes using the RESTful API Interface.&#x20;

This method is designed for developers seeking to enhance privacy and security in decentralized applications. By utilizing the scanshieldedtrc20notesbyovk RPC protocol, users can efficiently query and retrieve shielded transaction details, ensuring confidentiality and integrity.&#x20;

The interface is engineered to be intuitive yet powerful, catering to both novice and experienced developers in the blockchain space. With comprehensive documentation and support, integrating this method into your Web3 applications is seamless, promoting a secure and efficient blockchain ecosystem.

## Supported Networks

The ScanShieldedTrc20NotesByOvk REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

None: This method does not require any parameters.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using ScanShieldedTrc20NotesByOvk

Request

```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/ \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "value": "f34f1c799700a9d83b67fdcadd7be697010a8dbbcd520de4ac46a648e3e7ae3d"
}
```

Response

```json
{
  "Error": "class org.tron.core.exception.BadItemException : request requires start_block_index >= 0 && end_block_index > start_block_index && end_block_index - start_block_index <= 1000"
}
```

## Body Parameters

Here is the list of body parameters for the `scanshieldedtrc20notesbyovk` method:

1. **start\_block\_index**: The starting block index for the scan. It must be greater than or equal to 0.
2. **end\_block\_index**: The ending block index for the scan. It must be greater than the `start_block_index`.
3. **ovk**: The outgoing viewing key used to scan shielded TRC20 notes.
4. **limit**: The maximum number of notes to return. It must be a positive integer.
5. **offset**: The number of notes to skip before starting to collect the result set. It must be a non-negative integer.
6. **include\_spent**: A boolean indicating whether to include spent notes in the response.
7. **include\_unspent**: A boolean indicating whether to include unspent notes in the response.

## Use Cases

Here are some use-cases for the `scanshieldedtrc20notesbyovk` method:

1. **Privacy-Preserving Transactions**: In the realm of Web3 programming, privacy is a crucial aspect for many users and applications. The `scanshieldedtrc20notesbyovk` method can be used to facilitate privacy-preserving transactions by enabling users to interact with TRC-20 tokens without revealing their transaction history. This method allows developers to create applications where users can send and receive tokens with enhanced privacy, ensuring that their financial activities remain confidential.
2. **Secure Token Management**: For decentralized applications (dApps) that require secure token management, the `scanshieldedtrc20notesbyovk` method can be employed to manage shielded TRC-20 notes. This is particularly useful for applications dealing with sensitive financial data or requiring compliance with data protection regulations. By leveraging this method, developers can ensure that token transfers are not only secure but also shielded from public view, thus enhancing the overall security posture of the application.
3. **Enhanced User Anonymity**: In decentralized finance (DeFi) platforms, user anonymity is a significant concern. The `scanshieldedtrc20notesbyovk` method can be utilized to enhance user anonymity by allowing transactions to be conducted in a manner that obscures the identities of the parties involved. This capability is especially beneficial for users who prioritize anonymity in their financial dealings, as it helps protect their identity and transaction details from being exposed on the blockchain.

## Code for scanshieldedtrc20notesbyovk

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "value": "f34f1c799700a9d83b67fdcadd7be697010a8dbbcd520de4ac46a648e3e7ae3d"
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

## Common Errors

When using the ScanShieldedTrc20NotesByOvk HTTP REST API Tron method, the following issues may occur:

* **Incorrect OVK format**: The provided Outgoing Viewing Key (OVK) may be improperly formatted. Ensure that the OVK is a valid hexadecimal string and matches the expected length requirements.
* **Insufficient permissions**: The API request may fail if the associated account does not have the necessary permissions. Verify that the account has the appropriate access rights to perform shielded TRC-20 note scans.
* **Network connectivity issues**: Occasionally, network disruptions can prevent the API request from reaching the Tron network. Check your network connection and retry the request if a timeout or connectivity error occurs.
* **Outdated API version**: Using an outdated version of the API might lead to compatibility issues. Ensure that you are using the latest version of the Tron API to maintain compatibility and access new features.

The ScanShieldedTrc20NotesByOvk method offers significant benefits for Web3 applications by providing enhanced privacy features. It enables developers to efficiently scan and manage shielded TRC-20 transactions while maintaining confidentiality. This functionality is crucial for applications that prioritize user privacy and secure transaction handling in the decentralized ecosystem.

### Conclusion

The "ScanShieldedTrc20NotesByOvk" feature in the HTTP API for Tron provides a secure way to manage and verify shielded TRC-20 transactions. By leveraging this functionality, developers can ensure the privacy and integrity of transactions on the Tron network. This capability is crucial for applications requiring enhanced confidentiality and security in digital asset management.
