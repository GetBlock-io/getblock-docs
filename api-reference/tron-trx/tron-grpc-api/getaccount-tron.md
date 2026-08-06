---
description: >-
  Example code for the GetAccount gRPC method. Complete guide on how to use
  GetAccount gRPC method in GetBlock Web3 documentation.
---

# GetAccount - Tron

This method returns the on-chain data for an account, including its TRX balance in SUN, permissions, TRC-10 asset balances, and staking state.

## Service

This method is served by the `protocol.Wallet` and `protocol.WalletSolidity` service.

{% hint style="info" %}
This is a read method. It is also served by `protocol.WalletSolidity`, which returns only confirmed, irreversible data. Use the Solidity service for balance and payment verification.
{% endhint %}

## Method

```protobuf
rpc GetAccount (Account) returns (Account)
```

## Request Message

`Account`

| Field   | Type  | Description                                |
| ------- | ----- | ------------------------------------------ |
| address | bytes | Account address as raw bytes (41-prefixed) |

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{"address": "41f0cc5a2a84cd0f68ed1667070934542d673acbd8"}' \
  go.getblock.io:443 protocol.Wallet/GetAccount
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import grpc
# Stubs generated from java-tron's api.proto with grpcio-tools
from api import api_pb2_grpc
from core import Contract_pb2, Tron_pb2

creds = grpc.ssl_channel_credentials()
channel = grpc.secure_channel('go.getblock.io:443', creds)
metadata = [('x-api-key', '<ACCESS-TOKEN>')]
stub = api_pb2_grpc.WalletStub(channel)

response = stub.GetAccount(Tron_pb2.Account(address=bytes.fromhex('41f0cc5a2a84cd0f68ed1667070934542d673acbd8')), metadata=metadata)
print(response)
```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
{% code title="example.go" %}
```go
conn, _ := grpc.Dial(
    "go.getblock.io:443",
    grpc.WithTransportCredentials(credentials.NewTLS(&tls.Config{})),
)
defer conn.Close()

client := api.NewWalletClient(conn)
ctx := metadata.AppendToOutgoingContext(
    context.Background(), "x-api-key", "<ACCESS-TOKEN>")

resp, _ := client.GetAccount(ctx, &core.Account{Address: addrBytes})
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`Account`

| Field             | Type            | Description                                  |
| ----------------- | --------------- | -------------------------------------------- |
| address           | bytes           | The account address                          |
| balance           | int64           | TRX balance in SUN (1 TRX = 1,000,000 SUN)   |
| frozenV2          | repeated        | Stake 2.0 staking positions by resource type |
| account\_resource | AccountResource | Energy-related resource state                |
| assetV2           | map             | TRC-10 asset balances held by the account    |

## Use Cases

* **Balance Display**: Show an account's TRX balance and assets
* **Staking State**: Read an account's Stake 2.0 positions
* **Permission Checks**: Read owner and active permissions
* **Account Activation**: Detect whether an address is activated

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
