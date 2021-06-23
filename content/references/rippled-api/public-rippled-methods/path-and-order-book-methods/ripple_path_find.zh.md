---
html: ripple_path_find.html
parent: path-and-order-book-methods.html
blurb: Find a path for payment between two accounts, once.
---
# ripple_path_find
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/RipplePathFind.cpp "Source")


`ripple_path_find`方法是[path_find method][]的简化版本，它提供了一个带有 [payment path](paths.html) 的单一响应，您可以立即使用。它在WebSocket和JSON-RPC API中都可用。然而，随着时间的推移，结果往往会过时。与其多次调用以保持更新，不如使用[path_find method][]在可能的情况下订阅持续更新。

尽管`rippled`服务器试图找到最便宜的路径或路径组合进行支付，但不能保证此方法返回的路径实际上是最佳路径。

**Caution:** 小心来自不受信任服务器的寻路结果。服务器可以被修改为返回不太理想的路径来为其运营商赚钱。服务器在重载情况下也可能返回较差的结果。如果您没有自己的服务器可以信任进行寻路，则应比较来自不同参与方运行的多个服务器的寻路结果，以将单个服务器返回较差结果的风险降至最低。

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 8,
    "command": "ripple_path_find",
    "source_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "source_currencies": [
        {
            "currency": "XRP"
        },
        {
            "currency": "USD"
        }
    ],
    "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "destination_amount": {
        "value": "0.001",
        "currency": "USD",
        "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"
    }
}
```

*JSON-RPC*

```json
{
    "method": "ripple_path_find",
    "params": [
        {
            "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "destination_amount": {
                "currency": "USD",
                "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                "value": "0.001"
            },
            "source_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "source_currencies": [
                {
                    "currency": "XRP"
                },
                {
                    "currency": "USD"
                }
            ]
        }
    ]
}
```

*Commandline*

```sh
#Syntax ripple_path_find json ledger_index|ledger_hash
rippled ripple_path_find '{"source_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59", "source_currencies": [ { "currency": "XRP" }, { "currency": "USD" } ], "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59", "destination_amount": { "value": "0.001", "currency": "USD", "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B" } }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ripple_path_find)

请求包括以下参数:

| `Field`               | Type                       | Description             |
|:----------------------|:---------------------------|:------------------------|
| `source_account`      | String                     | 在交易中发送资金的帐户的唯一地址 |
| `destination_account` | String                     | 在交易中接收资金的帐户的唯一地址 |
| `destination_amount`  | String or Object           | [Currency Amount][] 目标帐户将在事务中接收的。 **Special case:**  您可以指定`"-1"`（对于XRP）或提供-1作为`value`字段的内容（对于非XRP货币）。 这要求路径尽可能多地传递，同时花费不超过`send_max`（如果提供）中指定的金额。 |
| `send_max`            | String or Object           | _(可选)_ [Currency Amount][] 这笔钱会花在交易上。不能与`来源货币`一起使用。 |
| `source_currencies`   | Array                      | _(可选)_ 源帐户可能要使用的货币数组。数组中的每个条目都应该是一个JSON对象，其中必须有`currency`字段和可选`issuer`字段， 例如如何指定[货币金额][货币金额]。包含的源货币不能超过 **18** 种。默认情况下，使用所有可用的源货币，最多可使用 **88** 个不同的货币/发行人对。  |
| `ledger_hash`         | String                     | _(可选)_ 用于分类帐版本的20字节十六进制字符串(参见[分类帐][]） |
| `ledger_index`        | String or Unsigned Integer | _(可选)_ 要使用的分类帐的[分类帐索引][]，或用于自动选择分类帐的快捷方式字符串(参见[分类帐][]） |

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 8,
    "status": "success",
    "type": "response",
    "result": {
        "alternatives": [
            {
                "paths_canonical": [],
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
                            "issuer": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 1,
                            "type_hex": "0000000000000001"
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
                            "issuer": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rLpq4LgabRfm1xEX5dpWfJovYBH6g7z99q",
                            "type": 1,
                            "type_hex": "0000000000000001"
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
                            "issuer": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rPuBoajMjFoDjweJBrtZEBwUMkyruxpwwV",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        }
                    ]
                ],
                "source_amount": "256987"
            }
        ],
        "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "destination_currencies": [
            "015841551A748AD2C1F76FF6ECB0CCCD00000000",
            "JOE",
            "DYM",
            "EUR",
            "CNY",
            "MXN",
            "BTC",
            "USD",
            "XRP"
        ]
    }
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "alternatives": [
            {
                "paths_canonical": [],
                "paths_computed": [
                    [
                        {
                            "currency": "USD",
                            "issuer": "rpDMez6pm6dBve2TJsmDpv7Yae6V5Pyvy2",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rpDMez6pm6dBve2TJsmDpv7Yae6V5Pyvy2",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rfDeu7TPUmyvUrffexjMjq3mMcSQHZSYyA",
                            "type": 1,
                            "type_hex": "0000000000000001"
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
                            "issuer": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "raspZSGNiTKi5jmvFxUYCuYXPv1V8WhL5g",
                            "type": 1,
                            "type_hex": "0000000000000001"
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
                            "issuer": "rpHgehzdpfWRXKvSv6duKvVuo1aZVimdaT",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rpHgehzdpfWRXKvSv6duKvVuo1aZVimdaT",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        }
                    ]
                ],
                "source_amount": "207414"
            }
        ],
        "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "destination_currencies": [
            "USD",
            "JOE",
            "BTC",
            "DYM",
            "CNY",
            "EUR",
            "015841551A748AD2C1F76FF6ECB0CCCD00000000",
            "MXN",
            "XRP"
        ],
        "status": "success"
    }
}
```

*Commandline*
```json
{
   "result" : {
      "alternatives" : [
         {
            "paths_canonical" : [],
            "paths_computed" : [
               [
                  {
                     "currency" : "USD",
                     "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                     "type" : 48,
                     "type_hex" : "0000000000000030"
                  }
               ]
            ],
            "source_amount" : "5212"
         },
         {
            "paths_canonical" : [],
            "paths_computed" : [
               [
                  {
                     "account" : "r9vbV3EHvXWjSkeQ6CAcYVPGeq7TuiXY2X",
                     "type" : 1,
                     "type_hex" : "0000000000000001"
                  },
                  {
                     "account" : "rnx1RNE5cJbYzMsJbF3XzyQMxZNBPqdCVd",
                     "type" : 1,
                     "type_hex" : "0000000000000001"
                  }
               ],
               [
                  {
                     "account" : "r9vbV3EHvXWjSkeQ6CAcYVPGeq7TuiXY2X",
                     "type" : 1,
                     "type_hex" : "0000000000000001"
                  },
                  {
                     "account" : "ragizZ31TmpachYAuG3n56XCb1R5vc3cTQ",
                     "type" : 1,
                     "type_hex" : "0000000000000001"
                  }
               ],
               [
                  {
                     "account" : "r9vbV3EHvXWjSkeQ6CAcYVPGeq7TuiXY2X",
                     "type" : 1,
                     "type_hex" : "0000000000000001"
                  },
                  {
                     "account" : "r9JS9fLbtLzgBCdFCnS3LpVPUBJAmg7PnM",
                     "type" : 1,
                     "type_hex" : "0000000000000001"
                  }
               ],
               [
                  {
                     "account" : "r9vbV3EHvXWjSkeQ6CAcYVPGeq7TuiXY2X",
                     "type" : 1,
                     "type_hex" : "0000000000000001"
                  },
                  {
                     "account" : "rDc9zKqfxm43S9LwpNkwV9KgW6PKUPrT5u",
                     "type" : 1,
                     "type_hex" : "0000000000000001"
                  }
               ]
            ],
            "source_amount" : {
               "currency" : "USD",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0.001002"
            }
         }
      ],
      "destination_account" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
      "destination_amount" : {
         "currency" : "USD",
         "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
         "value" : "0.001"
      },
      "destination_currencies" : [
         "USD",
         "015841551A748AD2C1F76FF6ECB0CCCD00000000",
         "BTC",
         "DYM",
         "CNY",
         "EUR",
         "JOE",
         "MXN",
         "XRP"
      ],
      "full_reply" : true,
      "source_account" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
      "status" : "success"
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`                  | Type   | Description                              |
|:-------------------------|:-------|:-----------------------------------------|
| `alternatives`           | Array  | 具有可能路径的对象数组，如下所述。如果为空，则没有连接源帐户和目标帐户的路径。 |
| `destination_account`    | String | 将接收付款交易的帐户的唯一地址 |
| `destination_currencies` | Array  | 表示目标接受的货币的字符串数组，如`"USD"`等3个字母的代码或40个字符的十六进制代码 `"015841551A748AD2C1F76FF6ECB0CCCD00000000"` |

 `alternatives` 数组中的每个元素都是一个对象，表示从一种可能的源货币（由发起帐户持有）到目标帐户和货币的路径。此对象具有以下字段:

| `Field`          | Type             | Description                            |
|:-----------------|:-----------------|:---------------------------------------|
| `paths_computed` | Array            | 定义[路径](paths.html)的对象数组 |
| `source_amount`  | String or Object | [Currency Amount][] 源必须沿着此路径发送，目的地才能接收所需的量|

The following fields are deprecated, and may be omitted: `paths_canonical`, and `paths_expanded`. If they appear, you should disregard them.

## Possible Errors

* Any of the [universal error types][].
* `tooBusy` - The server is under too much load to calculate paths. Not returned if you are connected as an admin.
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `srcActMissing` - 请求中省略了`source_account`字段。
* `srcActMalformed` - `source_account` 字段格式不正确。
* `dstActMissing` - 请求中省略了`destination_account`字段。
* `dstActMalformed` - `destination_account` 字段格式不正确。
* `srcCurMalformed` - `source_currencies`字段格式不正确。
* `srcIsrMalformed` - 请求中一个或多个货币对象的`issuer`字段无效。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
