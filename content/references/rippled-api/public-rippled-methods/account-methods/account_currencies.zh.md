---
html: account_currencies.html
parent: account-methods.html
blurb: Get a list of currencies an account can send or receive.
---
# account_currencies
[[Source]](https://github.com/ripple/rippled/blob/df966a9ac6dd986585ecccb206aff24452e41a30/src/ripple/rpc/handlers/AccountCurrencies.cpp "Source")

`account_currencies`命令根据帐户的信任行检索帐户可以发送或接收的货币列表(这不是一个完全确定的列表，但可以用来填充用户界面。）

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "command": "account_currencies",
    "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "strict": true,
    "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
    "method": "account_currencies",
    "params": [
        {
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "account_index": 0,
            "ledger_index": "validated",
            "strict": true
        }
    ]
}
```

*Commandline*

```sh
#Syntax: account_currencies account [ledger_index|ledger_hash] [strict]
rippled account_currencies rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn validated strict
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#account_currencies)

请求包括以下参数:

| `Field`        | Type                       | Description                    |
|:---------------|:---------------------------|:-------------------------------|
| `account`      | String                     | 帐户的唯一标识符，通常是帐户的[地址][]。|
| `ledger_hash`  | String                     | _(可选)_ 用于分类帐版本的20字节十六进制字符串(参见[分类帐][]） |
| `ledger_index` | String or Unsigned Integer | _(可选)_ 要使用的分类帐的[分类帐索引][]，或用于自动选择分类帐的快捷方式字符串。 (参见 [Specifying Ledgers][]) |
| `strict`       | Boolean                    | _(可选)_ 如果`true`，则`account`字段只接受公钥或XRP分类帐地址。否则，`帐户`可以是机密或密码短语（不推荐）。默认值为`false`。 |

以下字段已弃用，不应提供: `account_index`.

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "result": {
        "ledger_index": 11775844,
        "receive_currencies": [
            "BTC",
            "CNY",
            "DYM",
            "EUR",
            "JOE",
            "MXN",
            "USD",
            "015841551A748AD2C1F76FF6ECB0CCCD00000000"
        ],
        "send_currencies": [
            "ASP",
            "BTC",
            "CHF",
            "CNY",
            "DYM",
            "EUR",
            "JOE",
            "JPY",
            "MXN",
            "USD"
        ],
        "validated": true
    },
    "status": "success",
    "type": "response"
}
```

*JSON-RPC*

```json
200 OK
{
    "result": {
        "ledger_index": 11775823,
        "receive_currencies": [
            "BTC",
            "CNY",
            "DYM",
            "EUR",
            "JOE",
            "MXN",
            "USD",
            "015841551A748AD2C1F76FF6ECB0CCCD00000000"
        ],
        "send_currencies": [
            "ASP",
            "BTC",
            "CHF",
            "CNY",
            "DYM",
            "EUR",
            "JOE",
            "JPY",
            "MXN",
            "USD"
        ],
        "status": "success",
        "validated": true
    }
}
```

*Commandline*

```json
{
   "result" : {
      "ledger_hash" : "F43A801ED4562FA744A35755B86BE898D91C5643BF499924EA3C69491B8C28D1",
      "ledger_index" : 56843649,
      "receive_currencies" : [ "USD" ],
      "send_currencies" : [ "NGN", "TRC" ],
      "status" : "success",
      "validated" : true
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`              | Type                       | Description              |
|:---------------------|:---------------------------|:-------------------------|
| `ledger_hash`        | String - [Hash][]          | (May be omitted) The identifying hash of the ledger version used to retrieve this data, as hex. |
| `ledger_index`       | Integer - [Ledger Index][] | The ledger index of the ledger version used to retrieve this data. |
| `receive_currencies` | Array of Strings           | Array of [Currency Code][]s for currencies that this account can receive. |
| `send_currencies`    | Array of Strings           | Array of [Currency Code][]s for currencies that this account can send. |
| `validated`          | Boolean                    | If `true`, this data comes from a validated ledger. |

**Note:** 账户可以发送或接收的货币是基于对其信托额度的检查来定义的。如果一个账户有一个货币的信托额度，并且有足够的空间增加其余额，它就可以收到该货币。如果信托额度的余额可以下降，账户就可以发送这种货币。此方法不检查信任行是[Frozed](freezes.html)还是已授权。

## Possible Errors

* Any of the [universal error types][].
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `actNotFound` - 请求的`帐户`字段中指定的地址与分类帐中的帐户不对应。
* `lgrNotFound` - `ledger哈希`或`ledger索引`指定的分类帐不存在，或者确实存在，但服务器没有它。


{% include '_snippets/rippled-api-links.md' %}
