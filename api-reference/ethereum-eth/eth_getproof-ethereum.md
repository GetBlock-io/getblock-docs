---
description: >-
  Retrieve account and storage values, including Merkle proof, using
  eth_getProof. Essential for verifying on-chain data integrity and enabling
  trusted responses in IoT and mobile applications.
---

# eth\_getProof - Ethereum

{% hint style="success" %}
This method returns the account and storage values of a specified account, including the Merkle proof.
{% endhint %}

The eth\_getProof method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. The eth\_getProof method is particularly useful for scenarios where IoT devices or mobile apps, which are unable to run light clients, need to verify responses from untrusted sources using a trusted block hash.

The eth\_getProof RPC Ethereum method is an essential tool for verifying on-chain data integrity, enabling the retrieval of account details and storage proof data. This allows developers to create applications that can validate the state of specific accounts and their corresponding storage slots, ensuring accurate and trustworthy information. The method also facilitates the retrieval of proof data via different API endpoints, making it highly versatile.

### Supported Networks

Ethereum eth\_getProof RPC method supports the following network types:

* Mainnet
* Testnet: Sepolia, Hoodi

### Parameters

* DATA: The 20-byte address of the account or contract for which to retrieve proof data.
* ARRAY: An array of 32-byte storage keys to generate proofs for.
* QUANTITY | TAG: An integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter

### Request

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request, send a JSON object with the jsonrpc, method, and params fields. Below is an example of how to make a request using curl:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": [
        "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95",
        [
            "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"
        ],
        "latest"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getProof",
"params": ["0x0a8156e7ee392d885d10eaa86afd0e323afdcd95", ["0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"], "latest"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the proof details of the specified account and storage keys. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "accountProof": [
            "0xf90211a05b5baadd6f6f54b6c2ff681027fcc7bd9c02295d8b48508a51ce26f653fdac83a01264e15262bcc2622a7ec83f50bcb6e7f22333a20ba9b52d313c8bf4ad5d52c0a0f2d1e19358158b04b9ad32f4ce1610ea6fbc5c917db9f60b774d7bebdf71cfdba0d5c7a0aee1195989d8a8304b204208de7013d6c2ac0659e47e5e62791794967ea09203b2e0a425853315c767dedfa49af5fedd65b29ba77bb4731486c3e2d3ae2ca0c80038dd09540300d064ac6c175a4a72ac1f307fc97bdbe8577cdc5e3f7a3f97a0a700307810c1103353f24a276a466ec6e34f13662f6d67c61f88a3674aa730aaa00505a77fbe5dec5a366278b89e8b170529194635f6a9335cbf81bc965aa6838aa044062de1327e6a66677e9fe7cfdbcebb74de933342107a5cdbc274654ee5aa58a0b12e4f0c16968035c270970b9e89ea9bbc9ab1769c2a2cef3d47e55d3087f00ca06db6ecd27f4b7bea9c2601f6d832b8d26a0057fbd8adae8dee74a03b30668ffda0547615c66bc7d67b7b054ab62d4d5c55cfe5e586258f7e0a419f666d717576b0a0e13032a1a09cff2c3b4c9cfe3f2a31959f7b3af205e70b1b3af4ad2d938710dea07619b09eb43b1dcb4a094ea3208aa0f6ef25993d1fa79485582a9040847d5556a03a424f699ea8019a48bb427f874ea5c63eb00b51e0d990de1524c34d01268d76a099026adb9da129b368144735eb566ef8717fb48e8853a380878d4a042686819080",
            "0xf90211a048da2019a431473ae411205253ff3ed5f380941196f0c8b21f97fc27d91dd93ca0575e03b16d97c5c9fa38e5a96f50c9024f20a422cb67ba45b0d63032c872bcc8a0ad28969bc90174946203dcc858dc8cadae2613bead9676e772adc9c9c30428c8a0296bd5646f4b4a7e78ce1ab6b8c28d63c5f9195342fd251d7e57538fc0268231a0178fe264a92317cf4470e7ba46b66a7503c9ca96cc7d59d1b68e837317d42fa9a0eb5fbd5ad83690d29aa154dcababf3b576c366a2150870c72d40d2f6f714582da0a58cd0d3eabee46f9a171e96343ecde71f73af87c15a58231c5b4f1141ec013ca0767b5fb94bbba32633040dc805bc13f4966801c4a6673a5efb5b1b7b2ec6b515a0a57d5a239a816d47d18b9d0e5fe503cf0abf1172280d0a787f82f3e5b12c6949a010c95739a3216750aced606bd4cc8a46933d857aca245adafae95f2f8d86816da024921c0b234d653b120ace48ca22c74227bf06d017de4bc23425173cd545ebe5a074212a44aecfe03ccdc60706bef22e501e6824af4da7681a33d212ab27ba08caa09e3b5382792146ff72f0f755327c4c6d4a0cce838ecab38aec1d12a038a7e297a0bad880c3efec63dd215781aff56fe3ea794ecbbb0ac4b732ad4bfb8f76f7abbba0381edd48a2cec0bf03dbdf421da51db5c42a7ce21e400042f46623f3092d46aea0bf0ed13ab34eeefdd488d29c6de0d7e3fd89c6314532e976184ed4bfe75c93bc80",
            "0xf90211a09cc8d27c921beddf004d2ea9a3564ae0b3323d09dc10aae398c5d8ff7fb703cda0c1f15c55babb811210d417a4b25c765e548774089a9c23b39fb3bebb09ebe899a0c703079d00713620c16f62c9dbbd83c81585f38c5ac0ce2cfd37df7078da5ec7a05efdaf82292a207327474e89698767ee815ea49a873e5412b19806877a2f6e5da0d7c475c80a3ee7766c9888607007957ee8ace62d3aeec0ea1fd64b1cb0b3a6cea032e920d08bd88ec3d9afafe939397167acbc9af6a6635fcc2d9505e37878c9eba0c7f898c80e9a9bbc630914a8ceecd1601a53d92bf18db451c1ba9fd43124350ba07abc271af375a37d597df536f2d213202fb5e65e7644f47b92140ee4a9233cf1a07f87bb08c159e2ea9d6901a1565dffcc76c84eeb2a90380b205231ff4f1fbe84a0f82d2e57ed9d2df91fcf3879aefd8d46d5bcf9a1a7d77edb7671eceab224c0caa0a01fa10ba1690ac9e37e5559267fdbb971f9e7187055c3ee0123c05ba502b591a0311575afe6dcc540e98db576b4428f97b4816de0bd9677367914bb69c0b4094ba0924e7f826e7a2c1d58a04b61ebcc9deaa99c36e1db63516a7dcb7764ddf40feba00695d25d3787189ed5d2a2ea2326d738145e3e12609ee147ea1f66ee0d43a548a0036b89b8e50c164318cd12e0d70111aa8ba8509ac6db35069717331553135c7ca0c3f01ea87e0ce2f6344c6e9618f6315da8f274cf4d7cec68eaad19bdd987a47180",
            "0xf90211a02c6d9b233df5556ae59493cf7d1d57bd7227280dcb66609876744e94bd5c8aa9a075ebd25fc7e09c95d1df341c882fca0eaca0f12cf5e3dcc401bf89e3148bc207a0beab2f03b9d23d8fa158a602463206185f5fec4e5b7b3a12a551b5e852f91e65a0d842e2fada5d7f443da9593eee10b47a83de9bda36e25a2af2fa438f1c3c5525a012fcbbc7d297d31f5614fae255ed9e1782e22094a79cac22a6348b3ded64fc26a039514a5222717cd7e940bedf3f3ff36b06434efbbd6bdaf91d903a8da061c696a0f38ed147dc8a8375862d1e33910282ca77b37afc2c0a4a63488c2f548eff4cf7a0d165ad695f44322a7964c3f7fddecef8b61fe33a774ed6d4715875cd445bcbbda07745a1f1ac477cc273c7eb94d4112438669f60a5172fcc3e84beb5f82660fbd3a013ec98296c959cb7794e2e40e3ba357d31f76bd24c7481b545c899f616bf3ac0a08f6c628cb20ca2f1a86b87479cce82e9b4691a143bbc6681edc452e88184f04da0bd8296801d0a67ca91f42349d68d2df0b32484f78a6274cc73da3932b8ff77e4a0e3bd49af7bb1fbecf78ea78b1e13ecd7f3e3ccaa20c8d6f783fdf06092268de4a0a3919720a8b259a83147357ee6437346a1c8e29d7df82f20c9eecf3d3b88b8a7a07dd32fc56cc60274fdc0d43dabc009d182313f45f2694a6bc9d97c5f9dcfd1d7a0103dfccc73fc51c92590ab8885bc24860e0102efe20568cfb87f22440132b0f680",
            "0xf90211a078494b54c24a364d0dfdbb7ab1a24df72aaa707bc23acf36073d03f7bcbe340ba0e1ff0f62347ea9b1c401ae89120cfb62b37a701f07bb142dff2a798a34a9d555a033e4a074b8af79950e95513ad93cd5b721088e17c3d44513cfb81c3f3dc9ebdfa0730f8d949fb755930fa957ab8cf6ba073a1e3a676027ec88e38888fa76d827bda07a9ad74ed86231f0a6a85f7e7a548ba26a76afbcea27ceb21d1a5ecd4bc6bc1da0fa3c6c57e8debdd26d1aabb841c6c03408158c2f0f6e51f3010f58cff10cb852a04ea3c360628192c4e7c981d0e6301190103de581cf8d01770b73bf9559b205ada014bd05306f3a3b63f2120dea3b1e4b76638086187f773d47d67bfc5ee84bfab0a0731a8c8396fcc67db5fd00420c387da4f0ab289f9aecf5a585ecef22006dfd7fa0c8c20e5c209d580831f1f0dbd04adaa311477e321eb97b4ee4b3a9ed9e6d8a78a0d21e56ceedab25045a24af7a0faed25f2bfca7f5a8681b126a0b03a154c37256a0e54d18f3f09fd269750e25dd8d46485ddeada80915d52ab3a5691c4519f756d3a013865bddb81714df7a4923f2798ff9ee01504c517f0caf2045096caa59c9aad4a0d4a143f0dca73d72e57197b04747ff73d1d188dc9de39b36407f6f2de59293cca08d35b0fe293a24020d6ef4244cd22ac4d7c63b46decd7beff9d89bfe216e8afaa0f6f304d60ce1083e54c7d0b68392eb4ca07b180b11819c801a981d914364a59f80",
            "0xf90211a00ddd120e42ab56dffdfc91b5b6f04dc678927399aaf95e17c61a15351c3dff1ea09faf924e9f715ec1da992b1f95de02e4b57be3ef76eb37205199da5d25bf9390a0679368315ad01620edf30256f119ee5304983c3c483d4ef2078d92f7e0a50352a0f67af658e9948c8bf4a6d55205ed7efb14715af23342b9e1bcb4f9f621baf556a0af82f7842b20e0b5231bb720ab75421cc0c2a959cf173a40bb48ee39d94ea9e3a07e5927e0dc4e53b16f06ee71c24a623d067eebf80236ece7999333414612c138a01634bfa19c42da9fa050030eae6075f8601fa6561e400e1a66cd2f1866fb6b79a0260bf82785d7bae7287978ebe3abd9cd5e9fb82bbf33803f3bf9d226715bd537a019c9577ddd96b1ee4f1593862d363feaffa71f7141b78132255678f4828f1f5ca05dd1588b29ff0ffbe4e11c8d5bd3bbff533a6564c65dd790a0c0105485b770a9a0a35cb3ee22108a8d0c114b6d5a96466cf05b5918874030bcb3f6867fa1bb1d51a0bdb5a2fd8e46c728182c1e68cf1db569359d63b2d527d68ef786251c89c6101ca0d667d570c6fc185ccbde0fe7c57844858c6f897aad36f8e6bc7b22bcd77269afa0e39f212ffb93299e5b9f1f5a96153e2e53c709c5b6317e73e6c053e48ffaece3a0931b4edaa0fd97910dc2d911ad3b4059abe71774fa12c36ea46a66de07eae1c1a089a8cc758e91a89b1ae152230701d93e042b8d8b141f9893e342f85ac4f693eb80",
            "0xf9011180a0a542e234ee78a97f7a77cad158095b7c4be639763e988089c0b9bb6a7abd9bdf8080a04af6635e3b4c861fd7bd0f513101c9a1e23a2b7dc2430ff14cf9d5e66646e022a02bba3cf90021964df085b12cf313daa81f9998c1c0946ab55703525808ce9d67a0a9f1897a314b8536ed30706e7ead95b1c550cbaaec0ad83b07ca8850d1f953c4a010025023ce106abb7111c4c3dddf29e0e917e6cc4380346722b967e9e85ac881a004b6878a88c7d2af4a054eb869749461a7fa1af9420cfc4af56000eff4fa607c808080a053e37adc725e934b366cdc7e639dc8fdfcb005d9e26b20de26589cab92729132a0ba4c2daa0c33cd8633b4491f27628cf8f37774b84a3de687ae3402cc46d5c3e4808080",
            "0xe21fa048bdc55cb0f8afcaab21a807bd4d68642d2002b874dc031fc7bc210ffc9239d1"
        ],
        "address": "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95",
        "balance": "0x0",
        "codeHash": "0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
        "nonce": "0x0",
        "storageHash": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "storageProof": [
            {
                "key": "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
                "proof": [],
                "value": "0x0"
            }
        ]
    }
}
```

### Response Description

* accountProof: The Merkle proof for the account, represented as an array of strings.
* address: The address of the account or contract for which the proof was requested.
* balance: The current balance of the account in Wei.
* codeHash: The hash of the code of the account.
* nonce: The nonce of the account, representing the number of transactions sent.
* storageHash: The storage root hash of the account.
* storageProof: An array of storage proofs for the specified storage keys, including the key, proof, and value.

### Use Case

The eth\_getProof method is particularly useful for verifying on-chain data, especially in cases where applications need to validate the state of a specific account or storage slot. This is crucial for IoT devices, mobile apps, and other lightweight clients that cannot run full or light nodes. By using the eth\_getProof RPC method, these clients can verify the integrity of data received from untrusted sources. In case of an eth\_getProof error, developers should check the provided parameters and ensure that the block hash or address is correct. Additionally, by using different endpoints, developers can access the method across various environments.

The eth\_getProof method is also useful for transaction tracking. Developers can verify specific transactions associated with an account by retrieving the proof and cross-checking the values.

### Code Example

Below is an eth\_getProof example demonstrating how to make a request programmatically using Python.You can also make requests to the eth\_getProof method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_getProof",
    "params": [
        "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95",
        [
            "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"
        ],
        "latest"
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

// Replace <ACCESS-TOKEN> with your actual API key
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

// Request payload
const requestBody = {
  jsonrpc: '2.0',
  method: 'eth_getProof',
  params: [
    '0x0a8156e7ee392d885d10eaa86afd0e323afdcd95', // Address
    ['0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7'], // Storage keys array
    'latest', // Block tag
  ],
  id: 'getblock.io',
};

// Axios POST request
axios
  .post(url, requestBody, {
    headers: {
      'Content-Type': 'application/json',
    },
  })
  .then((response) => {
    console.log('Proof Response:', response.data.result);
  })
  .catch((error) => {
    console.error('Error fetching proof:', error.message);
  });
```
{% endtab %}
{% endtabs %}

This Python script sends a request to the eth\_getProof method and prints the returned proof information. Make sure to replace \<ACCESS-TOKEN> with your actual API token.

The Web3 eth\_getProof method can also be used in Web3 libraries for Ethereum, providing an interface to access and verify blockchain data for various use cases, including transaction tracking, account verification, and data validation.
