---
description: >-
  eth_getProof JSON-RPC API Interface: Retrieve account and storage proof data
  efficiently in the BSC protocol for secure blockchain interactions.
---

# eth\_getProof - BNB Smart Chain

{% hint style="success" %}
The method retrieves Merkle proof for account and storage, verifying inclusion in the BSC state trie.
{% endhint %}

The `eth_getProof` method in the BSC protocol is a JSON-RPC API method used to generate a Merkle proof for a specific account and its storage. By leveraging the `eth_getProof` Web3 interface, developers can retrieve cryptographic proofs of account existence and storage values, which are essential for verifying data integrity.

As part of the `eth_getProof` RPC protocol, this method requires parameters such as the account address, storage keys, and a block number. The response includes the account's balance, nonce, code hash, and storage proofs. This functionality is invaluable for applications requiring secure and verifiable state information.

### Supported Networks

The `eth_getProof` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_getProof` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Address**
  * **Type**: String
  * **Description**: The address of the account for which the proof is being requested.
  * **Required**: Yes
  * **Example**: `"0x7F0d15C7FAae65896648C8273B6d7E43f58Fa842"`
* **Storage Keys**
  * **Type**: Array of Strings
  * **Description**: A list of storage keys for which the proof is requested.
  * **Required**: Yes
  * **Example**: `["0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"]`
* **Block Parameter**
  * **Type**: String
  * **Description**: The block number or block tag (e.g., "latest", "earliest", "pending") for which the proof is requested.
  * **Required**: Yes
  * **Default/Supported Values**: `"latest"`, `"earliest"`, `"pending"`, or a specific block number in hexadecimal format.
  * **Example**: `"latest"`

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_getProof` :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getProof",
  "params": [
    "0x7F0d15C7FAae65896648C8273B6d7E43f58Fa842",
    ["0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"],
    "latest"
  ],
  "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by `eth_getProof` upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "address": "0x7f0d15c7faae65896648c8273b6d7e43f58fa842",
    "accountProof": [
      "0xf90211a0bfe9bccbb85bfcf0ff0038b4d430ac9c596f379bd3f46572887b9eec9131d960a063b0d3672157eb2ba2768bb15897688528298acd959afcf16dcbc5703ad2a537a0a33c9956b80245db5b4960f8ddc2c00ccfdf036a962c7f2389dd58b98fbb9b27a08999cff1cf8bfac12b36864559d4f88f2bad80d6be5db12acdda4b9eae0867caa0b61d4ba5844ac12f2d98edd5824d697c8650f8a81b92ad6ef632737841ff3cb8a030cdb568b768d92653e8186f780ebc76ff3a16c8e8d174563dc736a5a4bc590fa06fee6b2ba3e74914c92a168437076b226a2739e27ad42c94babe6b3d6a250304a0d111513ddcbcc4081603e3453583943ecec8229ffdbf781ee71d89ffc2f101b8a099c3d43697b711ea0a212b73e8c75b0aad60647519392da219eb70b9aa9c9eeca048293899488e21963033624149f9c07885ff4c8d325ffd27ab4aa99459ccf9bba04da7be62fccb5937095a325240cdc9cd998e359fdd0e12d9e545c54a4c2b6721a09de3551c19ccd26368b4d482c8eceb652cf1c1a60c1143c77caeebdecd37580ea0f33f50c2b610930b35b3a8b4058f460a0a8d3bf4880eae273ccf9fab091d7427a0574282f72a67ebe431cd2858fdbea0cf4d638e66d6dd516e9c8cf7f2c7ab1873a0e1b06b3c8d777ce95da3fb660cae23e39cf5b088120abe35689ec8947638f54ca02b0f22a1bf1018131097d11184c16591b344e06aaca43341675bf8cb1936e4a680",
      "0xf90211a041810d5194268dd6011e884cf5ccd4edb00bba22fa58f7d23bd72ef499e4f24ca02360c812c8b1e96a6d8f472944fb27d68418ad9e6b9524368ea578bb36fdf85ba07ad34a9b42c1e9aa89877103a044bb18d7a92f77ddb559e5ae0fd521cd9e58e2a084ed5ddbba4f7af5125a67cbce0eb15f940f537e8adc2ba25024a9878206e01fa070fb394c53dc0e2ab91acdd4b5404a95bb277c825833332b9ee199ce207b9df2a084ec446c8e1a71b14ea9992457f8788034ca1a55b96012cbefd6c235429b373da0e9b9c570563ac7d9d69394f52977badecebfc713081326c88807dc1f8d2f32d3a00a80b665bc017c80cab411d10a83844dc84453e40a2868b5c2648245107171b2a0c17a09a8bfd95aa7db94aa9fd732c267fa0aa25e312dbc1f4faf162b69da20eaa02f112546900b770177c073cad5e149d42734ca13d931897522db8ab163428a18a09d90afcd53f55b59bfb95dff121b6022b24ce3dc48de09064f7ae474ded4a595a0989be6fcdb7995976a2aaa88501c3318377994878a22cf4212a1dfccbdf3d465a0eb80c75e55bf0be01932d6bb842f1fb0695f0ecaed02497e87c738990ed04ec6a0d0272fb7f08bc4bfb016945c57adc5704b342007b8fea74cd851e722b325a6ffa0807cc820dbbc4b927d3ae99db0fd8c1c940314e40b2b821300b15eeafb2598aaa0588eb6ddfcd7b6827e7a0266482585baa8456f2a22ad1a0bd7e83148b7769a5080",
      "0xf90211a089cbff13c51907b3e48350a6c43c2badbc269696ee9d852249cf8d6397a2f781a027c1faba15df7373f7cf9dc17b96dcbb6080b7ce337f2d6ebeefd5b31355ef03a0cb2cda52ab6f11394b2a62bede03c534fcf20e5e3451bc71116c83f17e42152ea05d4518215109a92061ac1ef6f7c278d12654d4c146f9740535a2f2fb7b35f189a08e59679aa1dd324b952cc8e0543f522087c8a53bc401ed2181d2d6bde80c8b18a014370c3f79e76847cd837641e0e74b08e42bfbcdaf1e0bf21f96f620c7395ef4a0302bcb1bf10dd4cfae72f849f8c5d80442cab4cef7644de4d6c6a8fb5b5c0437a01b116aff19015141ea831c2dbf3c34772f7db8d246be85ba6b7dfaa76943522da07c9132f4e9979ed5c65163e7a5ca81da61ef16ca72ac84f0f6e88e644d95f565a016021310eb0a6dbc35f062eaca8db98c6f37945441452fc1aec366030134f929a0df4214498e62838294c5b898d7b8be4cc3840f4c1ab20f25dad99ec20ab6cfdca0ad53038c1fba56366d0b6212beab66249b77d6bfb18c74310f9ff3bd3f49e390a020383099f2292ebb16b5e0857a2ea1fc94f177154c4650d90d7abb097f55c971a0bb3428f6537372d64d67866010709704b6944803aa3e113fe067232e00df5ae9a0fe2293e393455c7dfb3fff695c5ead261d9984600108dd7add58f041b465a3c2a0f8db7b9b834a7faf3c6d5a4a8b83748d00c09791d2333ada853547ce0c99e82480",
      "0xf90211a056139619acddf034f17f13c7e255e721c554139654f14ea362da9117d9565054a0421d5270efe6ea7ed35b55bf648195585b08d2e5a2b267015718983e109a7065a053180dfca3fc7531a9125801bb8ecd5cf1dc973e56f41d16f9f5b58f8c304c97a0899ef6ff608f4d7bbb8aeea6b86a08ceb4fd625c8bc3f12e52311d5dd45ebefba0c7c183830a65317992d74b22b8ca403f914812afc035be7a9e48e5f3a26324a3a0f12301e3ad940a4a4dc01e4442da6792b0ec745aafe24020d22df8b688b99753a036efbdd9f6e4720956f00fffb0224c1a97e5f0453a57657279e6df4e3f416cfca07849c1407c03d7b487ae49d9aa7554caffca84e592389c8f78b69b3a96f0033ca0bcf1a926ff04cf367df5932b09f3c171e47d5eb9002e6228861d378c7dd59d24a03ac9dad42273ec3c3cac6d113995a4535ee14c000ef7795433a605f336d1fa41a06a62584037cb81afb834b216cbe89248254a5d06be4b072cd31f56730fef96e1a0daf0e90f59522919624da5f4e8d69e44032090906d852e37c228bf90c23fd602a094a090cc993cccfa8e374f116600d1d6a6ec8c9d12d50c0bb43ff63d8429f9c6a0c7b972521bbdc868164697792051ce61b8e838af05148fe716ecbb7a3431b2f6a0a257468b9307bec423a574caf27e0bf7421c7a5c93f0dae52894f5e4d2f43daea00db8a8840873bd27edbe3e3254202fc42eb04d90207cf28622533b69eb65542380",
      "0xf90211a0c7460a3ecbb03ea85c77b835464f9a292bf314d10f4543c009daed53cb457121a014c2f8eb291c608929e0843d04d8cd1cab857409f868a4a1d8095f571237f53da069525e4b615ac255820396520aa8a428aa3da8be9e5397423e1cf6ed05ad7f8aa0e8fb861df3f3fa783b4159c8d88bcddfdb2b41dfabc0a8fd40b32aa89042004ca0d7e52f07a990b77d9067b387f3e00190bb01ec31878c165935b6b3f2a443112da07bf9af5c8c6b9bfd4fdfbf0abdb6da8a5d2c133420784a1d95ff4cae65a41de4a09e96ba7fc51a9ad136e0ac43455fa1ce13c9df77a3f2bba1754b626cfa0e9937a031abb11635f5d5897ebbd6d41da45f88bdfbf0ed4ef6ae699b6756e687c2ea0ca0ee3a08c031f7547d7ab68e0ef1955a129494f87bf0caa06f78bdbd323dc39522a05ee7327e01cda33524fdcfec62e02e9caa7cd2386c1cfeb1dd0d7d239f22ea18a03a09b8b1a31e6317b87a8fd6e57e01e4c536a931360d3b1f939d882087068544a0e1ec547964e83daa3c58d0041290ab445f231a5115fa8ec49f02230494c24006a01dafbca5eb9b14112b176eb94697b51171e0fae34f758d0b908ba37033742722a0519cbdb41c2aa378bd100bcf7c77e1a65c3720b28d907d9f1d13bde19f9d6941a0e5cf1c6eb5f0575caea75ad5a7ae2069a18bc9906e738cbb0ea0760cfe819b20a07fa263ada9ff13aecace11a9f23da654267ff3dea5cd4689dfac830e228eef1880",
      "0xf90211a0f43295cd4b5238e879481358219260dfd17f22aa26a8003be5a9833a791ce4a4a087ab8bf2035b15023ea82daec97d85390a35a112f695260318d4c885430f72f5a07a7e8117b00b23a66de227087fc7c1d546b6c95afcf91e75162282220d3323aba0c2a7644324c068744cdcdef4d0bd98cf81be5d9a82dce5c404b3695214411395a08f9d472701038d73f76e5ded92a9cfdf66b7bd6c7fe5dbcb09169a9abba6053ea05e6afbe87b2cb12046ebd65d4c48c036b0e9dcd68ee2fa2c810f2a39f507f649a04ef7237c31856a7416ffbb50f1c9bc353d0a7b776b2b4df9080b993c3d0085d4a0bd0de598a6b5d1ecc93af9e7d94b6e6be2caf3ab21891b45dd439f175c94e38aa05fedd9483f8de0ee04eb3cc601bd8ed09e4d91f5e6398b18111b1d12470370bda0fedef497b803c8a132aee2b1e8b5bc6e6a083ea414df6067302783d9a0dfe8a3a04b4bf118e1cb471d12784734ac93bcc4b7bff6d5a846da7e93f9ded5e3b4223ea0cbd2d2227a35d49db8bab8f406657ffa458a0ca1578357f6dd723bc2cfbd0e72a05af282c8e9d2fdd2fe622c1129dd7e1639bb122e1080b64d8dba43926af60841a07d700b12b964e38ea3fc0cda175df6b0ac11dc4529196b75a30eb56a9bf0225aa0849721c3cd6178f5be29e1e270038dbe0f2fcacf26882e3126959a6882f24705a0ee45190612f5e1bf4529285d8d23725125da091265db7766bbe5fc04b6399edb80",
      "0xf90191a041983aec5aa5a334e7d0ebc3703fc12d379b2a57cbcdca303be090037560dce6a0875cd118da82dc61a0e707bc9b108d0a2e60bca4cedbcf000833227ada10922e80a01862aaac738b52299a01d1e0b43c2b398822714d0974897b59d71b7cacfd4e0280a0447c3aeea44619e3061bc485f90a1988f131956ba6e346a7f4a5511b9d388187a06341ad033dc4f39be8f1edb8f921687abd0ba565e2b6e1cc88ea00da79129125a0b1a0d6213237fbc3c263bbbc38e13d9389f55a3cbc20f6f919957896374a675fa03309c3e7c6e7271066e11199e2e18cdf90de7ba87bbd14d4a44bcb28644e23e180a098ef082eeea4fa1fec1495adf9b16f51b95351240b38d6751d834a260d6b7cc9a0583e7440895d88a9aaca54d0fd4ea4b13dfb3eeed77119efdf2d4370883fde04a0c8e39ce3f1e68c5a71d0a7f668a4b1edeed91d5895bf6153180a7867325563d6a0f00c21406bf1ec8bd2a0f1964d39111c1fc3cd5526b3682b45492dfee3f520ec80a0e7ea1677ef6c566290f48372659a1169d0c47382648acd3d7c73928e7eab388580",
      "0xf8518080a0e0f3947a2376b26ef9551eef631d020d1dce0d306e38164f2ec71973cfd160e380808080808080808080a0ce02fccf3dc3e79794826ccc86ca0e46657d39bd3934844fc89c70ffe31357ed808080"
    ],
    "balance": "0x0",
    "codeHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "nonce": "0x0",
    "storageHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "storageProof": [
      {
        "key": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "value": "0x0",
        "proof": []
      }
    ]
  }
}

```

### Body Parameters

Here is the list of body parameters for the `eth_getProof` method:

1. **address**: The Ethereum address that the proof is being requested for, e.g., `"0x7f0d15c7faae65896648c8273b6d7e43f58fa842"`.
2. **accountProof**: An array of strings representing the Merkle Patricia Trie proof for the account. Each string is a hex-encoded RLP-encoded node.
3. **balance**: The balance of the account in wei, represented as a hex string, e.g., `"0x0"`.
4. **codeHash**: The hash of the account's code, represented as a hex string. If the account is not a contract, this will be the hash of an empty string, e.g., `"0x0000000000000000000000000000000000000000000000000000000000000000"`.
5. **nonce**: The nonce of the account, represented as a hex string, e.g., `"0x0"`.
6. **storageHash**: The hash of the storage of the account, represented as a hex string, e.g., `"0x0000000000000000000000000000000000000000000000000000000000000000"`.
7. **storageProof**: An array of objects, each containing:
   * **key**: The storage key being proved, represented as a hex string.
   * **value**: The value at the storage key, represented as a hex string.
   * **proof**: An array of strings representing the Merkle Patricia Trie proof for the storage key. Each string is a hex-encoded RLP-encoded node.

### Use Cases

Here are some use-cases for `eth_getProof` method:

1. **Account and Storage Verification**: The `eth_getProof` method is used to obtain the Merkle proof for a given account and its storage keys. This is particularly useful in scenarios where a decentralized application (dApp) or a smart contract needs to verify the existence and state of an account or its storage on the Ethereum blockchain without relying on a full node. By providing a proof, developers can ensure that the state they are interacting with is authentic and has not been tampered with.
2. **Light Client Implementation**: In the context of light clients, which do not store the entire blockchain data, `eth_getProof` can be used to validate transactions and account states efficiently. Light clients can request proofs for specific accounts or storage keys to confirm their current state, enabling them to operate securely and effectively with limited resources.
3. **Auditing and Compliance**: For auditing purposes, `eth_getProof` can be employed to provide cryptographic evidence of the state of an account or contract at a particular block. This is useful for compliance and regulatory requirements where proving the state of a blockchain entity at a specific point in time is necessary. Auditors can use the proof to verify that the data they are examining is accurate and trustworthy.

### Code for eth\_getProof

{% tabs %}
{% tab title="Python" %}
```python

import requests
import json
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
  "jsonrpc": "2.0",
  "method": "eth_getProof",
  "params": [
    "0x7F0d15C7FAae65896648C8273B6d7E43f58Fa842",
    ["0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"],
    "latest"
  ],
  "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)

```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  "jsonrpc": "2.0",
  "method": "eth_getProof",
  "params": [
    "0x7F0d15C7FAae65896648C8273B6d7E43f58Fa842",
    ["0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"],
    "latest"
  ],
  "id": "getblock.io"
};

axios.post(url, payload, {
  headers: { "Content-Type": "application/json" }
})
.then(response => {
  console.log("Result:", response.data.result);
})
.catch(error => {
  if (error.response) {
    console.error("Error:", error.response.status, error.response.data);
  } else {
    console.error("Request failed:", error.message);
  }
});
```
{% endtab %}
{% endtabs %}

### Common Errors

When using the `eth_getProof` JSON-RPC API BSC method, the following issues may occur:

* Invalid address format: Ensure that the address is in the correct hexadecimal format and includes the `0x` prefix. Double-check for any typos or missing characters.
* Non-existent storage key: The specified storage key might not exist in the contract's state. Verify the key's correctness and ensure it is derived properly from the contract's storage layout.
* Incorrect block reference: If the block reference is invalid or not available, the method may fail. Confirm that the block identifier is correct and that it corresponds to an existing block in the blockchain.
* Network connectivity issues: Poor connectivity or network congestion can lead to timeouts or failed requests. Ensure a stable internet connection and consider retrying the request or using a different node provider.

The `eth_getProof` method is beneficial in Web3 applications as it allows developers to obtain Merkle proofs of account and storage states, enhancing the verification processes in decentralized applications. This functionality is crucial for ensuring data integrity and transparency, which are fundamental principles of blockchain technology.

### Conclusion

The `eth_getProof` JSON-RPC method is a powerful tool for retrieving account and storage proof data from the Ethereum blockchain, providing users with a way to verify the state of accounts. In the context of BNB Smart Chain (BSC), this method can be particularly useful for developers looking to ensure data integrity and perform validation checks. By leveraging `eth_getProof`, users can gain deeper insights into blockchain state and enhance their application's trustworthiness.
