# GetBlockByNum2

This method returns a block by its height, including its header and transactions.

## Service

This method is served by the `protocol.Wallet` and `protocol.WalletSolidity` service.

{% hint style="info" %}
This is a read method. It is also served by `protocol.WalletSolidity`, which returns only confirmed, irreversible data. Use the Solidity service for balance and payment verification.
{% endhint %}

## Method

```protobuf
rpc GetBlockByNum2 (NumberMessage) returns (BlockExtention)
```

## Request Message

`NumberMessage`

| Field | Type  | Description                  |
| ----- | ----- | ---------------------------- |
| num   | int64 | The block height to retrieve |

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{"num": 68000000}' \
  go.getblock.io:443 protocol.Wallet/GetBlockByNum2
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

response = stub.GetBlockByNum2(api_pb2.NumberMessage(num=68000000), metadata=metadata)
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

resp, _ := client.GetBlockByNum2(ctx, &api.NumberMessage{Num: 68000000})
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`BlockExtention`

| Field         | Type                          | Description                                |
| ------------- | ----------------------------- | ------------------------------------------ |
| blockid       | bytes                         | The block hash                             |
| block\_header | BlockHeader                   | The block header with number and timestamp |
| transactions  | repeated TransactionExtention | Transactions in the block                  |

## Use Cases

* **Block Explorers**: Render a block by height
* **Chain Indexing**: Stream blocks by height into a store
* **USDT Tracking**: Scan a block for TRC-20 transfers
* **Confirmation Context**: Read a block's time and producer

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
