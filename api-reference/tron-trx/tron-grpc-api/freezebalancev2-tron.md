---
description: >-
  Example code for the FreezeBalanceV2 gRPC method. Complete guide on how to use
  FreezeBalanceV2 gRPC method in GetBlock Web3 documentation.
---

# FreezeBalanceV2 - Tron

This method builds an unsigned Stake 2.0 transaction that stakes TRX to gain Energy or Bandwidth. The returned transaction must be signed and broadcast.

## Service

This method is served by the `protocol.Wallet` service.

## Method

```protobuf
rpc FreezeBalanceV2 (FreezeBalanceV2Contract) returns (TransactionExtention)
```

## Request Message

`FreezeBalanceV2Contract`

| Field           | Type         | Description                           |
| --------------- | ------------ | ------------------------------------- |
| owner\_address  | bytes        | Staking account address               |
| frozen\_balance | int64        | Amount of TRX to stake, in SUN        |
| resource        | ResourceCode | Resource to gain: ENERGY or BANDWIDTH |

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{"owner_address": "41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "frozen_balance": 1000000000, "resource": "ENERGY"}' \
  go.getblock.io:443 protocol.Wallet/FreezeBalanceV2
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

response = stub.FreezeBalanceV2(Contract_pb2.FreezeBalanceV2Contract(
    owner_address=bytes.fromhex('41f0cc5a2a84cd0f68ed1667070934542d673acbd8'), frozen_balance=1000000000,
    resource=Contract_pb2.ENERGY), metadata=metadata)
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

resp, _ := client.FreezeBalanceV2(ctx, &core.FreezeBalanceV2Contract{OwnerAddress: owner, FrozenBalance: 1000000000, Resource: core.ResourceCode_ENERGY})
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`TransactionExtention`

| Field            | Type           | Description                                         |
| ---------------- | -------------- | --------------------------------------------------- |
| transaction      | Transaction    | The built, unsigned transaction                     |
| txid             | bytes          | The transaction id                                  |
| result           | Return         | Build result, with a result flag and any error code |
| constant\_result | repeated bytes | Return data for constant (read-only) calls          |
| energy\_used     | int64          | Energy the call consumes                            |

## Use Cases

* **Gain Energy**: Stake TRX to obtain Energy for contract calls
* **Gain Bandwidth**: Stake TRX to obtain Bandwidth
* **Fee Reduction**: Reduce burned TRX by staking for resources
* **Validator Support**: Acquire TRON Power for voting

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
