---
description: >-
  Retrieve detailed or basic block information using a block hash. Fetch full
  block data or key details like transaction hashes, ideal for blockchain
  analysis or lightweight data retrieval.
---

# eth\_getBlockByHash - Ethereum

{% hint style="success" %}
The eth\_getBlockByHash method is part of the Core API in the Ethereum JSON-RPC retrieves information about a specific block using its block hash.
{% endhint %}

This method allows users to fetch either the full block data, including detailed transaction details, or just the basic block information with transaction hashes. This flexibility is beneficial for applications that require either comprehensive blockchain analysis or a lighter response focused on key block data.

### Supported Networks

The eth\_getBlockByHash method supports the following Ethereum networks

* **Mainnet**
* **Testnet**: Sepolia, Holesky

### Parameters

1. DATA, string (32-byte hash): The hash of the block to retrieve (required).
2. Boolean, true | false:

* true: Returns full transaction objects with complete transaction details.
* false: Returns only the transaction hashes.

### Request

URL

{% code fullWidth="false" %}
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}

To make a request to the eth\_getBlockByHash method, you can use the following curl command. The request sends a JSON-RPC query to the Ethereum network via the GetBlock API

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw {
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        false
    ],
    "id": "getblock.io"
}
```
{% endtab %}

{% tab title="wss" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getBlockByHash",
"params": ["0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7", false],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the block information. Below is an example response when full transaction objects are requested

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "difficulty": "0x76bffa240f156",
        "extraData": "0xd883010817846765746888676f312e31302e31856c696e7578",
        "gasLimit": "0x7a1200",
        "gasUsed": "0x79c0a3",
        "hash": "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        "logsBloom": "0x124334a4406810389873106b900344dc2c91908c0898000c8208a12040ca010080080d0100a000100602226387402285b6e016522e48490301d8e1a820ba0902e01493459022b0216884f24c80814111820c200443c0a05804048fc4c1428010000881001a810224c00ad0a4200c380204eabad0002544bb11f141714048020484a24100431689420814000481404c900c30116101512050010a7092020233700600ec404216200199c000604048be630151141481160001404213901c20211180147202898b0814081b469014202c088d312088010e211c200a82c2018ce0a120116626481b0a0d0d8280804944a9853184808a801208510498140e21906105",
        "miner": "0x09ab1303d3ccaf5f018cd511146b07a240c70294",
        "mixHash": "0xbf802f1c1aff805344df3d73ae005b1c21fa2e3475091c67a114a6585663c50b",
        "nonce": "0x0a6ac00001626799",
        "number": "0x768965",
        "parentHash": "0xbacb05fe267e4c7a8699c34dafe45af7d60253048ed1bdb00db3c66481c341b0",
        "receiptsRoot": "0x48ddd33fa5aa32e9fef8486b19997902da5c6f39bb6088190f6d6af3aa47739f",
        "sha3Uncles": "0x654afae449b771cef795bd94d8fb27f5f0064bb076eee651a2e434322cd94695",
        "size": "0x74e2",
        "stateRoot": "0x3e59f341a06d4a2513ff498ac40fa435f040d645be1c1ce02e71b683be0b2c53",
        "timestamp": "0x5cdcbbad",
        "totalDifficulty": "0x229ad3b36bcee81c393",
        "transactions": [
            "0x6bd9f57cde1c1518fff47ebbd9c505d06eeb4221b7c861a772f66c5970a358ad",
            "0xf0c969e381ccd3db03856991c83320b7f65b541ef1dbd1687bf6a66f0e8f68c2",
            "0xd8d81b0362c607493e737667a392182f9074f60f11d2df66c0b5d3ce00e0186b",
            "0xa96813cb6331b3fe4318c700aaf887d0d5d3bedd4b1b32e1dd0428e29934016c",
            "0x0344f710a33bc20f7543712c061bb04e494f2e588338b5dfbaf55ab1ea04823d",
            "0x97d4ea851a3bbb6cb4938b85ed975544831d427f5032a7db151f809052e82af1",
            "0x9d4540b91c59bead3e839d332861d8a46302a57112e51e7b734330fd9c786d14",
            "0xf3cd21af68ab1ef2097204cf9b64c1f66c9dc41e26d27d50f2bae0864d97bfd6",
            "0x7836705d1f92a9ecbff1a644925906119b6754ce4bae07852a2e7daf98c6dee8",
            "0x85dfcb0b50d0110506ee1a4ed2f05b2c3f028e23774886c4fb7409f0752bc3cf",
            "0x3fedeab94262602564e11d81c345440ba94c547e46e166b230d26414aa9f4070",
            "0xdbec943203cf7edc0a84fbe534ff0524a8b77ff70ddcd91737cd7398eead0c00",
            "0x80e9a183131981758ae53c994a9ac93ee091767c8e6a2b5fcab5812bfc5383de",
            "0x874ae8fbe82add30f02cfc5463cd16c80fff636318e843f83d976c3f59464541",
            "0x434b89713821b3efa2ec57eb90ed7756a78d3e5c4764d0e24d17675a3af11e9c",
            "0xba744eced4e18a9a7287b78e51ca3fb3a15b7a6a2b4d9946820ff81bec4b83d0",
            "0x37704014003040b30b5ff9d3bb8300186e3d781c01e0ea58806be56f0d0a1f70",
            "0xe7d09dc161b7ee7733b2013699d5699eaf1ea6757f84b34a22b6184c0059b256",
            "0x8d607ce8053a5d594e90034d67383f00a5dd9290f5bc018c4d3fb45595a625c0",
            "0xefd4473118b93d1ced5200642559c57be28226fcc9d0ee0f278e72ed9b7b3de9",
            "0x1b97a0eea41bbe147cc7782b9cdbbac78b8b8dcca878c7612406042f1b918348",
            "0x5f33d3dbde62cfbdd85f1210cd4f5b9cb8af63cb0bd19ab0de3e059c6fe50c18",
            "0xaab1c23312ac43c290c882df801870664d39414b8551f3fd3e915ca448b716d8",
            "0x807fc485600f09478a32af7b6382ff8ccb24de28cca110a5577455c543befbd1",
            "0x441896d272a4e06b6f99cd258cc217d0777b46a8335fa3958c3ada2ce3a6a011",
            "0x3c04df5e2ddd166d8b96a39ee9bbfa5e24c0785ff26d24af8832af200f848aa6",
            "0xfb2ac8f8ae559d7c2e709b4d69aa1d46a9e5f0aecd5768d44abc9f9467fe42c2",
            "0xbaebc501b74251cda19354b44a97a125219b80ef64048f99b166ceab716661cf",
            "0x761c72ea87c38f52dfbb3bf58712bd6b67abcd5ae1868b984f2a826d206783bd",
            "0xe78d1c617dae9847e329451396053fa58a0274b184eeaf63b69c4df96fc7ae9f",
            "0xdfb8733b54a4fd07b794ccdbbc74d9d2e98aafd5f89e634536e776aebf3572f3",
            "0xd324419133423ff97c76e4f5c1f2e48dea83d9c3d4126dc0a04ca81296427176",
            "0xd9162f3d9b9200463efaf87724f0fb68e2c8c9f95dc7e2b91d9bebccf1f1374d",
            "0x10f157abb2b55b30ed6459d144304bf08283a7c9fab65501453587773fb87182",
            "0x92696fab2030baf8cc59e501433a922c170331979b6d1458aaab9e3585b1ef5f",
            "0x97fc6b2ef7cd9646e1585781e61338309fedc30a5a2b7d7d5e0abda5dfe4559d",
            "0x819f49fc42ff3901adb3c2519edb34b85ecc14ef3aa2c1b56e92fbb5b89ca406",
            "0x23da2ef3ed7a803af60e600023f6b0486306a8b2a3973428dbcbc4bf1a6a9d12",
            "0xa209921f8eee60b749e19fe7d11596be927543fb8e628c8ecbbbb61d80a5eeae",
            "0x9b6c30ea9429e4ce0fa0d9a9374b645c50d5c78113dec8d0e5c53032bb85ad3e",
            "0x4116e97821023d88fb77f49be20b2bd7c090f3cf303b9926b86a20151839b953",
            "0xe15b73fc78e8253040d238f3d42f6a74281b32ffe3df54903b0c333edcc723f6",
            "0xcea3d9e319f83a6debf42b9004ed11967695f0e279d3ee5ccbb0a567cad65842",
            "0xc16c24a7cee736d878f775c637d301e7dfff085742fa35dfdcd75492582e6283",
            "0x235b8a530fa6ae23108988f39a8bc41bfcad02191c3b5b8d3a8e5cc16f6400b5",
            "0xf3de03bf5ef8f83daf9ac31bbc6e64f8b134407ac22cef47f0653caaa4f38630",
            "0x537510413f58d4ad2d7d797265a188d2704154723ecc3011f2a1a61051368243",
            "0x6e84d529202c029de140f8a18ed52900cf3e12a2bd7b94ad0b0c04fad652ebaf",
            "0xc3b2b09a41b10cbf36c61cb70184b509d8e3d66c673a4240c07ae40eec0b75dd",
            "0xa84967106806e0f8faa54acf45589c6623af7fbf413a529831f3d9199af2a3f5",
            "0x7e262c7ee07e069f0a076148fc137ee1c42bed1edc4e7dc09dcc417ba8d2e0c4",
            "0x9d1cbf00354abbebc191fbce18cb5182e71b165aa733dc9c1d7a60b556a621e1",
            "0x3b0fa1baa60ba1e5db74209824680baa5cf02b69db6a41b5804a28f8ae41d2b3",
            "0x4754fe0de14a45f8e3d63851879a375c616837cc7949b8b6097d1f49766be64a",
            "0xe2900927e7cc187f5902e489c13b44ad79ee0b56e1317e6a9ec2d847e421928f",
            "0x4adc09eb5cc918764d693cd854d25103343b58de061472701a9c7b06cc09abd6",
            "0x0ac62f12df0d1b4c3aa46b9d7b6e87b16e8c0b228eb10e4d7be9cf9f4f4a1870",
            "0x9d9e104b8efeb2e7230e6aa7d5544f27f779da5fa4699f8d2e463860535f03f3",
            "0x2c329261a25eb10006d8d044c0db648e7b16a18665b2feaa14606b90b2b55e5d",
            "0x5e7fa27938f8f7d1b935ba1de3a47e4da893649391cdfd297085ee42b897c76c",
            "0x82032aaf15e99ebeb3525622d192d43721df56fb0b712fc9ac1a99ce5a0c49e5",
            "0x739290e246bdd61d7cdb15dc88cfae8b64dbf1f06695aa1a20846a426d49d5ad",
            "0x16e6160cd09f1f19b3b2bb8a9e41e51db9a3846f650f68efde8fa1a602044852",
            "0xa36a09ccdcb2c5360de9266845679a3e50549388812ad7e38cefc2495fc7b071",
            "0x85bd4612d300661198adc80a58b2d4971a0f4240fd56aabff6f17ca6cb6cf3ef",
            "0x3ac5b9e4659d1b0025db7e6722c9b1052b2a61f7ed08abb2e153a589ac508074",
            "0x4d53a7f0c1778407d7b10acc227e4a520796c07da6cd001da0bb2435317df783",
            "0x78898ee9d6419db4fbaf55eb8e4b4e80f103d5e3f0a6cd1e92df229afc786067",
            "0x7be83ed50c1b436a5336a83daf216bdcbede5806858d1b8271677f70ebc0f5e8",
            "0x8e25894e329556911a1b08edc934c4b567a08688b5e6c1db058eaf245bae76e7",
            "0x45afe181c2094bee596d2f9bdd3c6d969de0bcfc573897023c0263403cb24509",
            "0x1320cfa0ccff8c977d4b65e5bc2fa7065df42239a6144874e505de5746c586c2",
            "0x121018a59b463005e13e3ea6e86f8793aadf8693120630e961b5857ceb6d14be",
            "0x67c26d7046ddff852ba77585b0d9a05af6a909d0bfb51d9fbee4dcbc5fd3600b",
            "0x12cf3c68991b3682e642091720e50dca36bbf59f46c37a419b2d8b85cfb476b4",
            "0x0203f086886d050726a7b30b75ae305a814bafa3a137012a6e18b46eb17a7a08",
            "0xa051777db75fb76834c2875b032238ba45a0f209990c50e0a0258af8ba509e2f",
            "0x89d74fc31d9ab834a07fde3c5b6c4f19e50154ea844ec60dc183baf7e9a6d2e2",
            "0x678c8125d6fa61c2936fade31445def419c36aa3a191f9ae54606737d61bb4c6",
            "0x66fd4b6a076885d73758bf09ad103aaddff51c53d445587d36015cd806a33cbf",
            "0x722e9cc4990b32ce7794886021c566d4891a3386ad6d33b991399ed5a630fa0e",
            "0xdcd17c430612450c75cbeb0fc913e39540a8cfc6d09100b8f7bb871ed524e3cf",
            "0xf280f1ee0e2874b8bb0e0fe0351b124190feb32294e8b54402c49ce448cc3ecc",
            "0x1c5d189214d7428f46aeeb5f9bbed3d0c0ec6ab03134dd49ccaedd40e2753287",
            "0xed626a966396085c30b92273a18646d515cfd44e925d33815776419011b6ae6f",
            "0x3759a3f962c2388c9b81d69fc73d8f6121764149ec4f6a073e7ada147dfb60e0",
            "0x1f279effe1f0b03480ca82f18f0e307938d62ba2c84c84d3775ead1df42f8f87",
            "0x9fe5f0e7306f05e853776077ddc5026ce10c5755c8a425d217e204cebafcd3b0",
            "0x9651e5d3e9fc45108aa2a8ad9cc531db1b9b477d6e8821f969df482764fcce3c",
            "0xf9a09e08a5191d7a96d34aab6106af1ec431b0f5b5820a937bac916bbf976c1d",
            "0xabf46268cc3a21f6a7f781cbc319087a72c2bc8dbf4e5f0e28cfbd8823d5b0c3",
            "0x638e8a99a9de922f10f5cbf10bd83940b126bf85ee7de99218ef6e4b80d0b08a",
            "0xf184ff0c2662d9ee0dc190d81ee1ccfaa5643e5511729d36c59577c35d040c9e",
            "0x846b230c49a6fb391f5ee6d2e8a1ae95e023c28903a67c903d8599b4c1be1c9d",
            "0x4a6b89da6ca3ff4a464d5d939788764111e3e5fd5da2c49353621f84c7eb798a",
            "0x8106060fbb940c5f2d813f0ffd23b6750e3e7d55f736a2458a9ffb3dd06d759e",
            "0x2caa3637b72abb31b82779f5145e835dfffdf20452f5c6481043fbd668cb0ca8",
            "0xf33a782c6398f0639f0d02b66a3966deac12290e720072f9134f791e0a5b4504",
            "0x77cb489a611566192e9a1ebeb9a5d7d8658057ecf22eb99516d8f7ec656b6d6d",
            "0x606c1cb822d9a58aae95bc477462c3afdee6c700ca0a8d0ac17a638426a90fb4",
            "0x0bba66422caf98286518334b3b308119a275cc7b66a843d9464d961c47ff9975",
            "0x49ab0669e55c90566831a0f82de9f833b7a0b34a0071b0ab292591d3be420399",
            "0x7dd1c678781291df7561e9cdfb5b1afe71f0173d98fb7026ca6f416b65d878b7",
            "0x301c8864770156b4fa20fe32a30cade8a4a75f8dbc7bb87510ecca8194ef99fd",
            "0x2fc423fb6f14b5a68c657dc727a312018bff0368bb64cc6bccfeb0f5cbe293aa",
            "0x1ee0bf102ee118aee15ebd8ceaa03ad0df56c088441378ab2942201beca9079c",
            "0xf2a3a209dc7efd66c691d9585928f31ed83d01b2d0d6906ddd79be8764c5c91c",
            "0xe5555d332961b11eb560decad8e3928b429352e9bb743da851d7fadf6c8da833",
            "0x88271c6510f3f2c388a24d9470ef1012310b23d3c8e17b5c4843edb234138775",
            "0xa254643649deb980efd3066fe531bc375fb8c484a1754cd46fde49c7dee5548b",
            "0xdead1172f5d7c3515daed53ae035e8067b28a1168f810d6d29bb0b17b5ee76a8",
            "0xa80af7330813241f136ea1bb0c64dbc964639a27069fce5e53207d61d0c5029f",
            "0xd37df83b91f75425aa620f7cfed026d2cbc082fb02b13b6579ec49773b03a541",
            "0xb6feafbda296aaf417fb67b48bdbd78305918d6d8100414bb648ef1bf3bfe825",
            "0x51b7219e988eb9c540f34c63cceedf874bb6316c8caa1bee8afa6d7c62ac73e7",
            "0x0d5b5e64c1500f626478a1704cd6ae999faa363f936976a79bb47ab807ce7c2c",
            "0xd1df9962d117eb70c2b196acd888f1e81de667df0b1d2580141a99d0098c8bf3",
            "0xe4cd230e903f0c5f13c3e8784c7d0f3c791653aafdc5f723a9057e74e09fd424",
            "0xe32b53e67bb5bb5efa77a42289b21d87e904baccf2860ed860a43de62bd09e49",
            "0xeb595d77c638fbb4173a2c3b8a51d8c80146f3aeebe67f346d0df3929073f029",
            "0x05ca3bb092b83ea74e6e0ff7342106f2ae822ba8098b1a2ceea56cbaa1b647d8",
            "0x52f7afc6a4c77158a4998a2325e8cd7fb6f4928f79b297ae14fd97ff79a1c641",
            "0x21edbd4fcea8f0df1a53d1f2e3cef44980557c59a02c6e3fb507dfe3bb4e44ce",
            "0xd8c01ff81a0766d7ccadc6f83d61810b72c945d5b69f421826e5dd5d3835c97a",
            "0xecc4c597e8c72e1d166298e0c2cca5e8f2eb6cf49526177e3cabd37296c05c62",
            "0x6b66ee8ef8bc512f825ec3211bf7be820155976a3a0fb8c07f7869ec9266cdd8",
            "0x108f3f51367e1e8d30c2db2da8eca0505a78d0637c697fee09dbd8491a75bc9d",
            "0x0c4ed1ce0f32b120a1e82788a9250432839f43b52cd635574c68b6c7139bbbab",
            "0xdb158256e5c30358787a2835aeae72522a6106c56f02b5343aa88c96a155db53",
            "0x85014c8e536522bac3895a7b508a57a9ae48defce8271abc76dfaa6d924bc8d5",
            "0xaf6e3f472e8ddc03e0b99db63e31abae5bc738720cd3515730bdb6d944c9b1d1",
            "0x384c8f2312a604b2cc4c8df10f1a6708bd5c37d113725722ee231e7a330fd1f9",
            "0x4df6aa3ef959afe0e6228f9c55f3c315e5bdb2fa2d1c3199dc0bc2613a0f2f9d"
        ],
        "transactionsRoot": "0x16998be3d0694d80ceb1ccf5474929383357f462fb1ce005988ce425c3321f2e",
        "uncles": [
            "0xc20189c0b1a4a23116ab3b177e929137f6e826f17fc4c2e880e7258c620e9817"
        ]
    }
}
```

### Response Description

* **id** (`string`): The identifier of the request, as sent by the client. In this case, `"getblock.io"`.
* **jsonrpc** (`string`): The version of JSON-RPC, here `"2.0"`.
* **result** (`object`): An object containing information about the block, with the following fields:
  * **difficulty** (`string`): The current difficulty of the block, in hexadecimal format.
  * **extraData** (`string`): Additional data related to the block, in hexadecimal format.
  * **gasLimit** (`string`): The gas limit for the block, in hexadecimal format.
  * **gasUsed** (`string`): The amount of gas used in the block, in hexadecimal format.
  * **hash** (`string`): The hash of the current block, in hexadecimal format.
  * **logsBloom** (`string`): The logs bloom filter for transactions in the block, in hexadecimal format.
  * **miner** (`string`): The address of the miner who mined the block.
  * **mixHash** (`string`): The hash used in the mining process of the block, in hexadecimal format.
  * **nonce** (`string`): A random value used by the miner to find the block hash.
  * **number** (`string`): The block number, in hexadecimal format.
  * **parentHash** (`string`): The hash of the parent block, in hexadecimal format.
  * **receiptsRoot** (`string`): The root hash of all receipts of transactions in the block, in hexadecimal format.
  * **sha3Uncles** (`string`): The hash of the list of all uncle blocks in the block, in hexadecimal format.
  * **size** (`string`): The size of the block in bytes, in hexadecimal format.
  * **stateRoot** (`string`): The root hash of the state after processing the block, in hexadecimal format.
  * **timestamp** (`string`): The timestamp of the block, in hexadecimal format.
  * **totalDifficulty** (`string`): The total difficulty up to and including the current block, in hexadecimal format.
  * **transactions** (`array of strings`): A list of transaction hashes included in the block.
  * **transactionsRoot** (`string`): The root hash of all transactions in the block, in hexadecimal format.
  * **uncles** (`array of strings`): A list of uncle block hashes associated with this block.

### Use Case

For developers and blockchain explorers, the eth\_getBlockByHash method is crucial for obtaining detailed information about specific blocks in the Ethereum blockchain. This method can be used for:

* Verifying transactions within a particular block using the transaction hash.
* Retrieving comprehensive block data for in-depth blockchain analysis.
* Debugging and investigating specific events on the Ethereum network by examining block data.

### Code Example

You can also make requests to the eth\_getBlockByHash method programmatically using Python. Below is an example using the requests library

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

# Define the API URL and access token
url = 'https://go.getblock.io/<ACCESS-TOKEN>/'
headers = {'Content-Type': 'application/json'}

# Prepare the request data
data = {
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": [
        "0xb3b20624c0870029c179a9610d55802dc8b651fdfebedb1653d1c06e165faf22",
        True
    ],
    "id": "getblock.io"
}

# Send the POST request
response = requests.post(url, headers=headers, data=json.dumps(data))

# Parse the JSON response
response_data = response.json()

# Print the result
print(json.dumps(response_data, indent=4))


```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const accessToken = '<ACCESS-TOKEN>';
const url = `https://go.getblock.io/${accessToken}/`;

const data = {
  jsonrpc: '2.0',
  method: 'eth_getBlockByHash',
  params: [
    '0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7',
    false
  ],
  id: 'getblock.io'
};

axios.post(url, data, {
  headers: {
    'Content-Type': 'application/json'
  }
})
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```
{% endtab %}
{% endtabs %}

This Python script sends a request to the eth\_getBlockByHash method and prints the returned block information. Make sure to replace with your actual API token. This method provides access to both Ethereum Mainnet and test networks like Sepolia and Goerli, enabling comprehensive blockchain analysis across multiple environments.
