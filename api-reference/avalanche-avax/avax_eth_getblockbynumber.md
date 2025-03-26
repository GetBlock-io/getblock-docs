---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - Avalanche

#### Parameters

`QUANTITY|TAG` - string

block number or "latest", "earliest" or "pending"

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getBlockByNumber",
  "params": [
    "latest",
    false
  ],
  "id": "getblock.io"
}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "baseFeePerGas": "0x5d21dba00",
        "blockExtraData": "0x",
        "blockGasCost": "0x61a80",
        "difficulty": "0x1",
        "extDataGasUsed": "0x0",
        "extDataHash": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "extraData": "0x000000000000000000000000004cf11400000000002a658c00000000000000000000000000000000000000000018f890000000000025b80600000000000d161100000000000000000000000000000000",
        "gasLimit": "0xe4e1c0",
        "gasUsed": "0x2c5572",
        "hash": "0x746540c469f195d7e44d124abcf406b7bd1ea8863eba18783dcd54791862e506",
        "logsBloom": "0x000002400000001800200028000800a10003400400002100200020020020025010004000404020430250040004180011010000040001000201b04200002034000441c000080800020c00000c00400009c08020000042000800020000c0880a000204009982000c010040800000a0180800000000088a5400020000114080002100024004040010000800000540004800100000112800001100000080100200000204404060006a00a004000440000000042000100118410004018020000100508080100204020800000001010110001000060040400004818401000202003000101080040000040400890401204800201220010000120042a10100000a400140",
        "miner": "0x0100000000000000000000000000000000000000",
        "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0000000000000000",
        "number": "0x1d612e5",
        "parentHash": "0xcba4335f04ed5b7a74f1199ec9eb80443dd798928a0fdd7a72bde5dbc218309a",
        "receiptsRoot": "0xddf770d794d2e146de33f3d35dc71658ab82287425c33f637ddf4c273dfd242f",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x2358",
        "stateRoot": "0xb1f578c894cf567bac5074e3cf48532b1ad79fbfd758c8962e630dfad6c9e064",
        "timestamp": "0x6479af6d",
        "totalDifficulty": "0x1d612e5",
        "transactions": [
            "0x98631d12bc5900df3e230d5fb7ef3bdfb3a322e160b4d870cf8aa89e0a45c36f",
            "0x43ba2405e8e150d191c1a8e87d25a87e0acfe4e7a0d46234c14999363b06aedc",
            "0x60883046aabd5ba51864364391b0c3f95a757633e156c91e59b3252fcf790718",
            "0xf04f8f608ebde9c4920a79e42765de824cd8af489536f86eddaae37b06bcc408",
            "0xd9e2d6660f069d8d1ee000338db384723d47dac3fbce43167000cb99a6346699",
            "0xdbfa076dde80dc8e554887c7168757e102b039a0208bd1cffe71df30dc46f610",
            "0x893b8a596daaa00ff3c8c1ea527fdfcf912d96dd14b9515d8edde15703dfd0fe",
            "0x2c3d0e39344ed0d3ef7b00a81d58f9da283695fc121c96a4f0581d71f801d5a9",
            "0xbcee7ada1ec471dc641b11fdf1b905ad70196b14b9b0d0a1f96a343208b73f51",
            "0x72bd7b396e1c0c8958372465ec0bcd6ea3766fa11e3f4cc14bfb07706af8aede",
            "0x9237e19cf9ac14f51cf996c785e6a72716d737d00fdcd72f6ef88ba15625f1a3",
            "0x488d58f1b762ca69a9728bada1ca5768a302e2d7ce8d4587775c6f9ffd20632a",
            "0xd4c0e048f204b4c715639fd7096b4ba42faadceb25262fb67df062d6bd4caf7b",
            "0xac6a969bfee4fc32e62f9be3e4c8e017288c89f70e09ab87417ed0437adcccd9",
            "0x55b6274a2601efa3f1a4d83f32ad1d60360c140a8043a4204a35c1a2b787a39f",
            "0x82a4f2c2af271f63ee23a4a73be3680cbab065936d2c7bb8f30876ada619f1ea"
        ],
        "transactionsRoot": "0x0133259bccb93f74041ebec5a24cb323f9351f2779a4c5b6ae8530d9c3a9f4c0",
        "uncles": []
    }
}
```
