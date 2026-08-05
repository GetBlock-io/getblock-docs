# DelegateResource

This method builds an unsigned transaction that delegates Energy or Bandwidth from staked TRX to another account, without transferring ownership of the stake.

## Service

This method is served by the `protocol.Wallet` service.

## Method

```protobuf
rpc DelegateResource (DelegateResourceContract) returns (TransactionExtention)
```

## Request Message

`DelegateResourceContract`

| Field             | Type         | Description                                              |
| ----------------- | ------------ | -------------------------------------------------------- |
| owner\_address    | bytes        | Account delegating the resource                          |
| receiver\_address | bytes        | Account receiving the delegated resource                 |
| balance           | int64        | Amount of staked TRX whose resource is delegated, in SUN |
| resource          | ResourceCode | Resource to delegate: ENERGY or BANDWIDTH                |
| lock              | bool         | Whether to lock the delegation for a fixed period        |

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{"owner_address": "41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "receiver_address": "41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "balance": 1000000000, "resource": "ENERGY"}' \
  go.getblock.io:443 protocol.Wallet/DelegateResource
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

response = stub.DelegateResource(Contract_pb2.DelegateResourceContract(
    owner_address=bytes.fromhex('41f0cc5a2a84cd0f68ed1667070934542d673acbd8'),
    receiver_address=bytes.fromhex('41a614f803b6fd780986a42c78ec9c7f77e6ded13c'), balance=1000000000,
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

resp, _ := client.DelegateResource(ctx, &core.DelegateResourceContract{OwnerAddress: owner, ReceiverAddress: receiver, Balance: 1000000000, Resource: core.ResourceCode_ENERGY})
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

* **Sponsor Energy**: Delegate Energy so another account can pay for calls
* **Fee Abstraction**: Cover users' contract costs from a resource pool
* **Exchange Operations**: Delegate resources to hot-wallet addresses
* **Resource Sharing**: Share Bandwidth across accounts

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
