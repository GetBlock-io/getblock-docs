# CreateTransaction2

This method builds an unsigned TRX transfer transaction from a sender, recipient, and amount. The returned transaction must be signed offline and submitted with BroadcastTransaction.

## Service

This method is served by the `protocol.Wallet` service.

## Method

```protobuf
rpc CreateTransaction2 (TransferContract) returns (TransactionExtention)
```

## Request Message

`TransferContract`

| Field          | Type  | Description                    |
| -------------- | ----- | ------------------------------ |
| owner\_address | bytes | Sender address as raw bytes    |
| to\_address    | bytes | Recipient address as raw bytes |
| amount         | int64 | Amount to transfer in SUN      |

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{"owner_address": "41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "to_address": "41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "amount": 1000000}' \
  go.getblock.io:443 protocol.Wallet/CreateTransaction2
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

response = stub.CreateTransaction2(Contract_pb2.TransferContract(
    owner_address=bytes.fromhex('41f0cc5a2a84cd0f68ed1667070934542d673acbd8'),
    to_address=bytes.fromhex('41a614f803b6fd780986a42c78ec9c7f77e6ded13c'), amount=1000000), metadata=metadata)
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

resp, _ := client.CreateTransaction2(ctx, &core.TransferContract{OwnerAddress: owner, ToAddress: to, Amount: 1000000})
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

* **TRX Transfers**: Build a native TRX transfer for signing
* **Offline Signing**: Produce an unsigned transaction to sign externally
* **Wallet Backends**: Construct transfers server-side before client signing
* **Batch Payments**: Build multiple transfers before broadcasting

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
