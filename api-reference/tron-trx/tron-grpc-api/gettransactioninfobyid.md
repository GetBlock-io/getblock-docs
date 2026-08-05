# GetTransactionInfoById

This method returns the execution result of a transaction: its block, Energy and Bandwidth used, fee, and smart-contract logs. It is the TRON equivalent of a transaction receipt.

## Service

This method is served by the `protocol.Wallet` and `protocol.WalletSolidity` service.

{% hint style="info" %}
This is a read method. It is also served by `protocol.WalletSolidity`, which returns only confirmed, irreversible data. Use the Solidity service for balance and payment verification.
{% endhint %}

## Method

```protobuf
rpc GetTransactionInfoById (BytesMessage) returns (TransactionInfo)
```

## Request Message

`BytesMessage`

| Field | Type  | Description                            |
| ----- | ----- | -------------------------------------- |
| value | bytes | The transaction id (hash) as raw bytes |

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{"value": "d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"}' \
  go.getblock.io:443 protocol.Wallet/GetTransactionInfoById
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

response = stub.GetTransactionInfoById(api_pb2.BytesMessage(value=bytes.fromhex('d5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62')), metadata=metadata)
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

resp, _ := client.GetTransactionInfoById(ctx, &api.BytesMessage{Value: txidBytes})
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`TransactionInfo`

| Field       | Type            | Description                                         |
| ----------- | --------------- | --------------------------------------------------- |
| id          | bytes           | The transaction id                                  |
| blockNumber | int64           | Block height of inclusion                           |
| receipt     | ResourceReceipt | Energy and Bandwidth used, and the execution result |
| fee         | int64           | Fee burned for the transaction, in SUN              |
| log         | repeated Log    | Smart-contract event logs emitted                   |

## Use Cases

* **Receipt Reads**: Confirm success and read cost
* **TRC-20 Events**: Read Transfer logs from a token contract
* **Energy Accounting**: Read Energy and Bandwidth consumed
* **Fee Reconciliation**: Read the fee burned

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
