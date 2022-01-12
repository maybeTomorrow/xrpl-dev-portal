---
html: path_find.html
parent: path-and-order-book-methods.html
blurb: Find a path for a payment between two accounts and receive updates.
---
# path_find
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/PathFind.cpp "Source")

*WebSocket API only!* The `path_find` method searches for a [path](paths.html) along which a transaction can possibly be made, and periodically sends updates when the path changes over time. For a simpler version that is supported by JSON-RPC, see the [ripple_path_find method][]. For payments occurring strictly in XRP, it is not necessary to find a path, because XRP can be sent directly to any account.

路径查找命令有三种不同的模式或子命令。使用`subcommand`参数指定所需的参数

* `create` - 开始发送寻路信息
* `close` - 停止发送寻路信息
* `status` - 获取当前打开的寻路请求的信息

尽管`rippled`服务器试图找到最便宜的路径或路径组合进行支付，但不能保证此方法返回的路径实际上是最佳路径。由于服务器负载的原因，寻路可能找不到最佳结果。此外，您应该注意来自不受信任的服务器的寻路结果。服务器可以被修改为返回不太理想的路径来为其运营商赚钱。如果您没有自己的服务器可以信任进行寻路，则应比较来自不同参与方运行的多个服务器的寻路结果，以将单个服务器返回较差结果的风险降至最低(**注意：**返回的结果不一定是恶意行为的证明；这也可能是服务器负载过重的症状。）

## path_find create
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/PathFind.cpp#L50-L56 "Source")

`path_find`的`create`子命令创建一个正在进行的请求，以查找可能的路径，通过该路径可以从一个指定帐户进行支付交易，从而使另一个帐户接收到所需的某种货币金额。初始响应包含两个地址之间的建议路径，这将导致接收到所需的量。之后，服务器将发送其他消息，类型为`"":"path_find"`，其中包含对潜在路径的更新。更新的频率由服务器决定，但通常意味着有新的分类帐版本时，每隔几秒钟更新一次。一个客户端一次只能打开一个寻路请求。如果另一个寻路请求已在同一连接上打开，则旧请求将自动关闭并替换为新请求。
 

### Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 8,
    "command": "path_find",
    "subcommand": "create",
    "source_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "destination_amount": {
        "value": "0.001",
        "currency": "USD",
        "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"
    }
}
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#path_find)

请求包括以下参数:

| `Field`               | Type             | Description                       |
|:----------------------|:-----------------|:----------------------------------|
| `subcommand`          | String           | 使用`"create"`发送create sub命令 |
| `source_account`      | String           | 要从中查找路径的帐户的唯一地址(换句话说，将要发送付款的帐户。） |
| `destination_account` | String           | 要查找路径的帐户的唯一地址(换句话说，将收到付款的帐户。） |
| `destination_amount`  | String or Object | 目标帐户将在事务中接收的[Currency Amount][]。  **Special case:** 您可以指定`"-1"`（对于XRP）或提供-1作为`value`字段的内容（对于非XRP货币）。 这要求路径尽可能多地传递，同时花费不超过`send_max`（如果提供）中指定的金额。 |
| `send_max`            | String or Object | _(可选)_ [Currency Amount][] 这笔钱会花在交易上。与`source_currencies`不兼容。 |
| `paths`               | Array            | _(可选)_ 对象数组的数组，表示要检查的[付款路径](paths.html)。您可以使用它来更新您已经知道的特定路径的更改，或者检查沿特定路径付款的总体成本。 |

服务器还识别下列字段，但不能保证使用这些字段的结果：`source_currencies，`bridges`。这些字段应视为保留供将来使用。


### Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 1,
  "status": "success",
  "type": "response",
  "result": {
    "alternatives": [
      {
        "paths_computed": [
          [
            {
              "currency": "USD",
              "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "currency": "USD",
              "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "currency": "USD",
              "issuer": "r9vbV3EHvXWjSkeQ6CAcYVPGeq7TuiXY2X",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "r9vbV3EHvXWjSkeQ6CAcYVPGeq7TuiXY2X",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "currency": "USD",
              "issuer": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ]
        ],
        "source_amount": "251686"
      },
      {
        "paths_computed": [
          [
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ]
        ],
        "source_amount": {
          "currency": "BTC",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.000001541291269274307"
        }
      },
      {
        "paths_computed": [
          [
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ]
        ],
        "source_amount": {
          "currency": "CHF",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.0009211546262510451"
        }
      },
      {
        "paths_computed": [
          [
            {
              "account": "razqQKzJRdB4UxFPWf5NEpEG3WMkmwgcXA",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "account": "razqQKzJRdB4UxFPWf5NEpEG3WMkmwgcXA",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ]
        ],
        "source_amount": {
          "currency": "CNY",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.006293562"
        }
      },
      {
        "paths_computed": [
          [
            {
              "account": "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "account": "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "account": "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ]
        ],
        "source_amount": {
          "currency": "DYM",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.0007157142857142858"
        }
      },
      {
        "paths_computed": [
          [
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "account": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "account": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ]
        ],
        "source_amount": {
          "currency": "EUR",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.0007409623616236163"
        }
      },
      {
        "paths_computed": [
          [
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ]
        ],
        "source_amount": {
          "currency": "JPY",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.103412412"
        }
      }
    ],
    "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "destination_amount": {
      "currency": "USD",
      "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
      "value": "0.001"
    },
    "id": 1,
    "source_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "full_reply": false
  }
}
```

<!-- MULTICODE_BLOCK_END -->

初始响应遵循[标准格式](response-formatting.html)，成功的结果包含以下字段:

| `Field`               | Type             | Description                       |
|:----------------------|:-----------------|:----------------------------------|
| `alternatives`        | Array            | 具有建议的[paths]（paths.html）的对象数组，如下所述。如果为空，则找不到连接源帐户和目标帐户的路径。 |
| `destination_account` | String           | 将接收交易的帐户的唯一地址 |
| `destination_amount`  | String or Object | 目标将在事务中收到的[Currency Amount][]  |
| `id`                  | (Various)        | （仅限WebSocket）WebSocket请求中提供的ID在此级别再次包含。 |
| `source_account`      | String           | 将发送事务的唯一地址 |
| `full_reply`          | Boolean          | 如果为`false`，则这是不完整搜索的结果。 稍后的答复可能有更好的途径。 如果为`true`，则这是找到的最佳路径(从理论上讲，仍然有可能存在更好的路径，但是`涟漪`找不到。） 在关闭寻路请求之前，每次关闭新分类帐时，`rippled`都会继续发送更新。|

`alternatives`数组中的每个元素都是一个对象，表示从一种可能的源货币（由发起帐户持有）到目标帐户和货币的路径。此对象具有以下字段:

| `Field`          | Type             | Description                            |
|:-----------------|:-----------------|:---------------------------------------|
| `paths_computed` | Array            | 定义[路径]的对象数组(paths.html) |
| `source_amount`  | String or Object | [Currency Amount][] 源必须沿着此路径发送，目的地才能接收所需的量 |

### Possible Errors

* 任何[通用错误类型][]。
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `noEvents` - 您使用的协议不支持异步回调，例如JSON-RPC(有关与JSON-RPC兼容的路径查找方法，请参阅[ripple_path_find method][]。）

### Asynchronous Follow-ups

除了初始响应之外，服务器还会以类似的格式发送更多消息，以便随着时间的推移更新 [payment paths](paths.html) 的状态。这些消息包括原始WebSocket请求的`id`，这样您就可以知道是哪个请求提示了它们，以及字段`"type": "path_find"`以指示它们是其他响应。其他字段的定义方式与初始响应相同。

如果后续包括`"full_reply":true`，则这是rippled在当前分类帐中可以找到的最佳路径。

下面是一个从路径查找创建请求进行异步跟踪的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 1,
    "type": "path_find",
    "alternatives": [
        /* paths omitted from this example; same format as the initial response */
    ],
    "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "destination_amount": {
        "currency": "USD",
        "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
        "value": "0.001"
    },
    "source_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59"
}
```

<!-- MULTICODE_BLOCK_END -->

## path_find close
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/PathFind.cpp#L58-L67 "Source")

`path_find`的`close`子命令指示服务器停止发送有关当前打开的寻路请求的信息。

### Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 57,
  "command": "path_find",
  "subcommand": "close"
}
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| `Field`      | Type   | Description                                |
|:-------------|:-------|:-------------------------------------------|
| `subcommand` | String | 使用`"close"`发送关闭子命令 |

### Response Format

如果路径查找请求已成功关闭，则响应的格式与对[`path_find create`](#path_find-create)的初始响应的格式相同，并加上以下字段:

| `Field`  | Type    | Description                                             |
|:---------|:--------|:--------------------------------------------------------|
| `closed` | Boolean | 值`true`表示此答复是对`path_find close`命令的响应。 |

如果没有未完成的寻路请求，则返回一个错误。

### Possible Errors

* 任何[通用错误类型][]。
* `invalidParams` - 如果指定的字段不正确，或者缺少任何必需的字段。
* `noEvents` - 如果您尝试在不支持异步回调的协议（例如JSON-RPC）上使用此方法。 （有关与JSON-RPC兼容的路径查找方法，请参阅[路径查找方法][]。）
* `noPathRequest` - 您试图在没有打开的寻路请求时关闭该请求。

## path_find status
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/PathFind.cpp#L69-L77 "Source")

`path_find`的`status`子命令请求立即更新客户端当前打开的寻路请求。

### Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 58,
  "command": "path_find",
  "subcommand": "status"
}
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| `Field`      | Type   | Description                                  |
|:-------------|:-------|:---------------------------------------------|
| `subcommand` | String | 使用`"status"`发送状态子命令 |

### Response Format

如果路径查找请求已打开，则响应的格式与对[`path_find create`](#path_find-create)的初始响应的格式相同，并加上以下字段:

| `Field`  | Type    | Description                                             |
|:---------|:--------|:--------------------------------------------------------|
| `status` | Boolean | 值`true`表示此答复是对`path_find status`命令的响应。|

如果没有未完成的寻路请求，则返回一个错误。

### Possible Errors

* 任何[通用错误类型][]。
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `noEvents` - 您使用的协议不支持异步回调，例如JSON-RPC(有关与JSON-RPC兼容的路径查找方法，请参阅[路径查找方法][]。）
* `noPathRequest` - 当没有打开的寻路请求时，您尝试检查寻路请求的状态。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
